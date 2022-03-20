# (Depreciated) RMRK 0.1 standard

The following **entities** are defined:

- [COLLECTION + Metadata](entities/collection.md)
- [NFT + Metadata](entities/nft.md)

The following **interactions** are possible:

- [MINT](interactions/mint.md) (Minting a collection)
- [CHANGEISSUER](interactions/changeissuer.md) (Changing the issuer of a collection)
- [MINTNFT](interactions/mintnft.md) (Minting an NFT inside a collection)
- [SEND](interactions/send.md) (Sending an NFT to a recipient)
- [LIST](interactions/list.md) (List an NFT for sale)
- [BUY](interactions/buy.md) (Buy an NFT)
- [CONSUME](interactions/consume.md) (Burn an NFT)

## Known Issues

- some NFTs published under this standard have been published without dedicated tools and are
  malformed.
  [Example](https://kusama.subscan.io/extrinsic/0xb8396caf702b3197cd6286b8d7424b255dd79e1e4d49e4a05ee66cae8d4bb3f3).
- this spec version had a bug in that it did not specify a standard version in the MINT and MINTNFT
  interactions. When the version is missing from the MINT, it should be assumed to mean 0.1.
