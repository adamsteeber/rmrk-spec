# MINTNFT

The MINTNFT interaction creates an [NFT](../entities/nft.md) inside of a
[Collection](../entities/collection.md).

## Standard

The format of a MINTNFT interaction is the same as for [MINT](mint.md):

`0x{bytes(RMRK::MINTNFT::{version}::{html_encoded_json})}`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `html_encoded_json` is the [HTML encoded](https://onlineutf8tools.com/url-encode-utf8) minified [NFT JSON](../entities/nft.md#examples)

## Examples

[Chunky RMRK Banner NFT](https://singular.rmrk.app/collectibles/11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039):

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

Became:

```
RMRK::MINTNFT::1.0.0::%7B%22collection%22%3A%22e0b9bdcc456a36497a-RMRKBNNRS%22%2C%22name%22%3A%22Chunky%22%2C%22instance%22%3A%22CHUNKY%22%2C%22transferable%22%3A1%2C%22sn%22%3A%220000000000000039%22%2C%22metadata%22%3A%22ipfs%3A%2F%2Fipfs%2Fbafkreifafxtjvsity6q656wz66eibusjillitzzj7obpa4o7i2xbajbib4%22%7D
```

[And was the 39th call in a batch, submitted as](https://kusama.subscan.io/extrinsic/0x583f1c31dfee5d9fbf74b551ed6bc820aa9bd8012306b959222b668435355ed8):

```
0x524d524b3a3a4d494e544e46543a3a312e302e303a3a253742253232636f6c6c656374696f6e2532322533412532326530623962646363343536613336343937612d524d524b424e4e52532532322532432532326e616d652532322533412532324368756e6b79253232253243253232696e7374616e63652532322533412532324348554e4b592532322532432532327472616e7366657261626c6525323225334131253243253232736e253232253341253232303030303030303030303030303033392532322532432532326d6574616461746125323225334125323269706673253341253246253246697066732532466261666b72656966616678746a76736974793671363536777a363665696275736a696c6c69747a7a6a376f627061346f3769327862616a62696234253232253744
```
