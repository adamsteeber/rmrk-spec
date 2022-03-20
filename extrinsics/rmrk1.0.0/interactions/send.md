# SEND

Send an [NFT](../entities/nft.md) to an arbitrary recipient.

You can only SEND an existing NFT that you own (i.e. one that has not been issued a [CONSUME](consume.md) interaction yet).

## Standard

The format of a SEND interaction is:

`0x{bytes(RMRK::SEND::{version}::{id}::{recipient})}`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [NFT](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields)
- `recipient` is the address of the recipient, e.g.
`CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`

## Effects

This interactions cancels the last [LIST](list.md) interaction on the NFT. It is equivalent to
having called LIST with a price equal to 0 (i.e. cancel listing).

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

The original owner `DQcegDuBQG6V99hgRd87UJ8anZxTcumJEVBAnAGomXCJ3dc` sent this NFT to `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp` by preparing the following SEND remark:

```
RMRK::SEND::1.0.0::6802213-24d573f4dfa1d7fd33-KAN-KANS-0000000000000001::CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp
```

[Then submitted it as](https://kusama.subscan.io/extrinsic/0xa71729fcc1c63890294aca99488ec34a2b2ff10b59cd30f8bb565a89d1b47189):

```
0x524d524b3a3a53454e443a3a312e302e303a3a363830323231332d3234643537336634646661316437666433332d4b414e2d4b414e532d303030303030303030303030303030313a3a43706a734c4443314a467972686d3366744339477334516f79726b484b685a4b744b37597147545246745461666770
```
