# LIST

A LIST interaction lists an NFT as available for sale at a specified price. The NFT can be purchased by anyone with a [BUY](buy.md) interaction after
a valid LIST interaction. A listing can be canceled and is automatically considered canceled when a user execute a valid [BUY](buy.md) interaction.

You can only LIST an existing NFT that you own (i.e. one that has not been issued a [CONSUME](consume.md) interaction yet).

## Standard

The format of a LIST interaction is:

`0x{bytes(RMRK::LIST::{version}::{id}::{price})}`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [NFT](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields)
- `price` is the listing price of the item, expressed in plancks (lowest denomination available on
chain), e.g. `1000000000000` for `1 KSM`. Alternatively, `price` can also be `0` which cancels the
listing.

To prevent price spoofing via rapid relisting, a LIST interaction must reign for **at least 5 blocks**
before a subsequent LIST interactions can be considered valid.

## Effects

An NFT's LIST is canceled by setting the price to `0`, or by the NFT being [sold](BUY.md),
[sent](SEND.md), or [consumed](CONSUME.md). While these interactions will **NOT** emit the appropriate
LIST interaction with a price set to `0`, they **IMPLY** the cancellation of a listing (see the `new` key value in the `changes` array in the [example](#examples))

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

The first listing of this NFT was made by `HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU` for 6.86 KSM by preparing the following LIST remark:

```
RMRK::LIST::1.0.0::11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039::6860000000000
```

[Then submitted it in bytes as the 14th call in a batch](https://kusama.subscan.io/extrinsic/0xeac12ca7408f01276628bbdc64ff9c4b5e38023ad924709e8663f849350dd9b7):

```
0x524d524b3a3a4c4953543a3a312e302e303a3a31313233383333312d6530623962646363343536613336343937612d524d524b424e4e52532d4348554e4b592d303030303030303030303030303033393a3a36383630303030303030303030
```

> Note: submitting a new LIST with a different price changes the listing price.

If the current owner lists an NFT and then decides that they want to keep it, they can cancel their listing by setting price to 0:

```
RMRK::LIST::1.0.0::11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039::0
```

Or in bytes:

```
0x524d524b3a3a4c4953543a3a312e302e303a3a31313233383333312d6530623962646363343536613336343937612d524d524b424e4e52532d4348554e4b592d303030303030303030303030303033393a3a30
```

A [BUY](buy.md) automatically cancels any active listing:

```
{
  "changes": [
    {...}, {...}, ...,
    {
    "field": "forsale",
    "old": "12005000000000",
    "new": "0",
    "caller": "FacsDhsHsv7rCUWfqsnnYSMfXDg2fRuyjAjd9eGCJRjoxh2",
    "block": 11251433,
    "opType": "BUY"
    }
  ]
}
```
