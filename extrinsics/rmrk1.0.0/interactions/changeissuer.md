# CHANGEISSUER

The CHANGEISSUER interaction allows a [collection](../entities/collection.md) issuer to change the
issuer field to another address. The original issuer immediately loses all rights to mint further
[NFT](../entities/nft.md)s inside that collection.

This is particularly useful when selling the rights to a collection's operation or changing the
issuer to a null address to relinquish control over it.

## Standard

The format of a CHANGEISSUER interaction is:

`0x{bytes(RMRK::CHANGEISSUER::{version}::{id}::{newissuer})}`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [collection](../entities/collection.md)'s ID.
- `newissuer` is the address of the new issuer, e.g. `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`

You can find an [online byte conversion tool here](https://onlineutf8tools.com/convert-utf8-to-bytes). Do not include spaces between bytes.

## Examples

Given a collection like:

```json
{
  "changes": [],
  "block": 9664708,
  "name": "RMRK profile banners",
  "max": 0,
  "issuer": "HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU",
  "symbol": "RMRKBNNRS",
  "id": "e0b9bdcc456a36497a-RMRKBNNRS",
  "metadata": "ipfs://ipfs/bafkreid6qxa5sasknkyig7d42xac5u2sjlngmkajxz4mbohk2z2bnhnvku"
}
```

If we want to change owner to `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`, we would compose:

```
RMRK::CHANGEISSUER::1.0.0::e0b9bdcc456a36497a-RMRKBNNRS::CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp
```

Which would be submitted as:

```
0x524d524b3a3a4348414e47454953535545523a3a312e302e303a3a6530623962646363343536613336343937612d524d524b424e4e52533a3a43706a734c4443314a467972686d3366744339477334516f79726b484b685a4b744b37597147545246745461666770
```

If submitted in block `10000000` the resulting collection would become:

```json
{
  "changes": [
    {
      "field": "issuer",
      "old": "HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU", 
      "new": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
      "caller": "HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU",
      "block": 10000000,
      "opType": "CHANGEISSUER"
    }
  ],
  "block": 9664708,
  "name": "RMRK profile banners",
  "max": 0,
  "issuer": "CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp",
  "symbol": "RMRKBNNRS",
  "id": "e0b9bdcc456a36497a-RMRKBNNRS",
  "metadata": "ipfs://ipfs/bafkreid6qxa5sasknkyig7d42xac5u2sjlngmkajxz4mbohk2z2bnhnvku"
}
```
