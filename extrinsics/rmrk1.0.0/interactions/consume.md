# CONSUME

The CONSUME interaction burns an [NFT](../entities/nft.md) and optionally specifies a purpose.

This is useful when NFTs are spendable like with in-game potions, one-time votes in DAOs, or concert tickets.

You can only CONSUME an existing NFT that you own (i.e. one that has not been issued a [CONSUME](consume.md) interaction yet).

## Standard

The CONSUME interaction can be a stand alone remark or can be issued in a [`utility.batchAll`](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions)
call with a supplimental remark specifying a purpose for burning. The format of the necessary `system.remark(A)` is:

`0x{bytes(RMRK::CONSUME::{version}::{id})}`

The format for the optional purpose `system.remark(B)` is:

`0x{bytes({reason})}`

Thus, the format for a CONSUME interation with a specified purpose is:

`utility.batchAll(system.remark(A), system.remark(B))`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [NFT](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields)
- `reason` is the [byte-encoded](https://onlineutf8tools.com/convert-utf8-to-bytes) burn reason specific to the issuing application, e.g.
`0x4841544348` for "HATCH". The reason is arbitrary; correct interpretation of the reason is the responsibility of the application.
If a CONSUME remark is submitted alone (i.e. not inside of a `utility.batchAll` call) the [Consolidator](https://github.com/rmrk-team/rmrk-tools) will render the `reason` simply as `true`.

## Effects

This kills the NFT, but does not erase it. An NFT's ledger of interactions will always consolidate up to the last valid interaction.
The CONSUME interaction cancels the last [LIST](list.md) interaction on the NFT. It is equivalent to
having called LIST with a price equal to 0 (i.e. cancel listing). It also will not allow any further interactions, e.g. [EMOTE](emote.md) or [SEND](send.md).

## Examples

Let's look at this Super Founder Kanaria egg with an ID of `6802213-24d573f4dfa1d7fd33-KAN-KANS-0000000000000001`:

```json
{
  "collection": "24d573f4dfa1d7fd33-KAN",
  "name": "Super Founder Kanaria egg",
  "instance": "KANS",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/bafkreigp3amcrr5urzntlyjpsvjccye4u7dvtyeatmlgzgecpy2kaczhka"
}
```

To hatch this egg, the owner `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp` was required to submit a CONSUME interaction with "HATCH" as the specified reason.

Thus, `system.remark(A)` was:

```
RMRK::CONSUME::1.0.0::6802213-24d573f4dfa1d7fd33-KAN-KANS-0000000000000001
```

In bytes:

```
0x524d524b3a3a434f4e53554d453a3a312e302e303a3a363830323231332d3234643537336634646661316437666433332d4b414e2d4b414e532d30303030303030303030303030303031
```

`system.remark(B)` was:

```
HATCH
```

or in bytes:

```
0x0x4841544348
```

[The final submission was](https://kusama.subscan.io/extrinsic/0xef8edd5377323941727d8f6aff063862d41e25f8827265ef7cfbf2528ef13dc9):

```
utility.batchAll(
[
  tx.system.remark(
    0x524d524b3a3a434f4e53554d453a3a312e302e303a3a363830323231332d3234643537336634646661316437666433332d4b414e2d4b414e532d30303030303030303030303030303031
  ),
  tx.system.remark(
    0x0x4841544348
  )
]
);
```

## How does an app detect a CONSUME?

The Kanaria egg app listened for `utility.batchAll` extrinsics that containted a CONSUME `system.remark` with an accompanying `system.remark` reason
that was "HATCH" or "0x0x4841544348." Once detected, instead of removing the NFT from the UI, the app displayed an animated image of the egg cracking its shell.
