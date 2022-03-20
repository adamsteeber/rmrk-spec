# MINT

The MINT interaction creates an [NFT collection](../entities/collection.md).

## Standard

The format of a MINT interaction is:

`0x{bytes(RMRK::MINT::{version}::{html_encoded_json})}`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `html_encoded_json` is the [HTML encoded](https://onlineutf8tools.com/url-encode-utf8) minified [collection JSON](../entities/collection.md#examples)

Convert the entire remark including the interaction prefix [into bytes](https://onlineutf8tools.com/convert-utf8-to-bytes), then submit it with a `0x` prefix.

## Examples

[RMRK profile banners Collection](https://singular.rmrk.app/collections/e0b9bdcc456a36497a-RMRKBNNRS):

```json
{
  "name": "RMRK profile banners",
  "max": 0,
  "issuer": "HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU",
  "symbol": "RMRKBNNRS",
  "id": "e0b9bdcc456a36497a-RMRKBNNRS",
  "metadata": "ipfs://ipfs/bafkreid6qxa5sasknkyig7d42xac5u2sjlngmkajxz4mbohk2z2bnhnvku"
}
```

Became:

```
RMRK::MINT::1.0.0::%7B%22name%22%3A%22RMRK%20profile%20banners%22%2C%22max%22%3A0%2C%22issuer%22%3A%22HeyRMRK7L7APFpBrBqeY62dNhFKVGP4JgwQpcog2VTb3RMU%22%2C%22symbol%22%3A%22RMRKBNNRS%22%2C%22id%22%3A%22e0b9bdcc456a36497a-RMRKBNNRS%22%2C%22metadata%22%3A%22ipfs%3A%2F%2Fipfs%2Fbafkreid6qxa5sasknkyig7d42xac5u2sjlngmkajxz4mbohk2z2bnhnvku%22%7D
```

[And was submitted as](https://kusama.subscan.io/extrinsic/0x76621cc94084448ab9aaed8c744b0dc96753288dd9be018a6d6bb8b4c2d5e66f):

```
0x524d524b3a3a4d494e543a3a312e302e303a3a2537422532326e616d65253232253341253232524d524b25323070726f66696c6525323062616e6e6572732532322532432532326d617825323225334130253243253232697373756572253232253341253232486579524d524b374c37415046704272427165593632644e68464b564750344a67775170636f673256546233524d5525323225324325323273796d626f6c253232253341253232524d524b424e4e525325323225324325323269642532322533412532326530623962646363343536613336343937612d524d524b424e4e52532532322532432532326d6574616461746125323225334125323269706673253341253246253246697066732532466261666b7265696436717861357361736b6e6b79696737643432786163357532736a6c6e676d6b616a787a346d626f686b327a32626e686e766b75253232253744
```
