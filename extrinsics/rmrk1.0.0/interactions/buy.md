# BUY

The BUY interaction allows a user to immediately purchase an [NFT](../entities/nft.md) that was listed for
sale using the [LIST](list.md) interaction, as long as the listing hasn't been canceled.

You can only BUY an _existing_ NFT that has been _listed_ (i.e. one that has not been issued a [CONSUME](consume.md) interaction and whose listed price is not 0).

## Standard

The BUY interaction is a
[`utility.batchAll`](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions)
call of a `system.remark` call and a `balances.transfer` call.
The transfer of tokens **MUST** be facilitated by a `balances.transfer` call.
The `balances.transferAll` and `balances.transferKeepAlive` calls **will not validate the BUY interaction**. 

- the first `system.remark(A)` is: `0x{bytes(RMRK::BUY::{version}::{id})}`
- the second transaction is a transfer of tokens to the current owner of the NFT for an amount that is _greater than or equal to_
the price advertised on the NFT's latest [LIST](list.md) entry.

The format of a BUY interaction is: `utility.batchAll(system.remark(A), balances.transfer({list_price}))`.

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [NFT](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields)
- `list_price` is the price advertised on the item's [LIST](list.md) entry

A BUY that satisfies the [LIST](list.md) conditions of an item's sale immediately counts as a
transfer of ownership. No [SEND](send.md) is necessary.

## Effects

This interactions cancels the last [LIST](list.md) interaction on the NFT. It is equivalent to
having called LIST with a price equal to 0 (i.e. cancel listing).

## Examples

Let's look at this [Chunky RMRK Banner NFT](https://singular.rmrk.app/collectibles/11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039):

```json
{
  "collection": "e0b9bdcc456a36497a-RMRKBNNRS",
  "name": "Chunky",
  "instance": "CHUNKY"
  "transferable": 1,
  "sn": "0000000000000039",
  "metadata": "ipfs://ipfs/bafkreifafxtjvsity6q656wz66eibusjillitzzj7obpa4o7i2xbajbib4"
}
```

[Which was listed with the 14th call in a batch](https://kusama.subscan.io/extrinsic/0xeac12ca7408f01276628bbdc64ff9c4b5e38023ad924709e8663f849350dd9b7)
by `HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU` (public key: `0xe0b9bdcc45111c7c23b354318607f4cbda8e7f017d500aa15402b1a16a36497a`) for 6.86 KSM:

```
RMRK::LIST::1.0.0::11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039::6860000000000
```

It was bought by `GRM82pGiZnL8RdAW6xH8d43T2xH1Lgvgd6itzvZxyqEwCtD` who prepared the following BUY remark:

```
RMRK::BUY::1.0.0::11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039
```

In bytes, that's:

```
0x524d524b3a3a4255593a3a312e302e303a3a31313233383333312d6530623962646363343536613336343937612d524d524b424e4e52532d4348554e4b592d30303030303030303030303030303339
```

To complete the purchase, `GRM82pGiZnL8RdAW6xH8d43T2xH1Lgvgd6itzvZxyqEwCtD`
[submitted a `utility.batchAll` call](https://kusama.subscan.io/extrinsic/0xa6a6d14f6572df6180658e88e207d3953d1bd5b50182f1eeccee58049a22b1ec)
comprised of two necessary transactions and one supplimental transaction:

```
utility.batchAll(
[
  tx.system.remark(
    0x726d726b3a3a4255593a3a312e302e303a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d30303030303030303030303030303031
  ),
  tx.balances.transfer(
    0xe0b9bdcc45111c7c23b354318607f4cbda8e7f017d500aa15402b1a16a36497a, 6860000000000
  ),
  tx.balances.transfer(
    0xd1bc4259aeb77874ee7ca72a9763d6385763068b56bf47fcabd0d854311ab7c1, 140000000000
  )
]
);
```

There was a third call in the `utility.batchAll` for a `balances.transfer` in the amount of 0.14 KSM because `GRM82pGiZnL8RdAW6xH8d43T2xH1Lgvgd6itzvZxyqEwCtD` used
[Singular](https://singular.rmrk.app/) to prepare the transaction which charges a small fee by tacking on an additional `balances.transfer` call to the Singular wallet.

----------------

It is an implementing library's responsibility to count BUYs without a sufficient transfer attached
as invalid. It is the buyer's responsibility to send the right amount or risk sending money to a
seller without actually getting the item.
