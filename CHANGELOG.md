## RMRK Specifications v2.0.0 - March 21st, 2022

Various superficial changes were made for the sake of consistency such as grammar, spelling, hyperlinks, and formatting.

### Creation of new directories and files. Moving and removing old directories and files.

- created [`/evm`](/evm), [`/pallets`](/pallets), and [`/general`](/general)
- replaced `/standards` with [`/extrinsics`](/extrinsics)
- moved and updated `./README.md` to [`/extrinsics/README.md`](/extrinsics/README.md)
- created [`./README.md`](./README.md) briefly describing the extrinsics, evm, and pallets standards
- created [`/general/README.md`](/general/README.md) describing the RMRK NFT paradigm
- replaced files in `/dumps` with .zip compressed directories containing full [`1.0.0`](/extrinsics/1.0.0) & [`2.0.0`](/extrinsics/2.0.0) unconsoidated & consolidated dumps
- moved `/dumps` to [`/extrinsics/dumps`](/extrinsics/dumps)
- moved and updated `/implementers-guide.md` to [`/extrinsics/implementers-guide.md`](/extrinsics/implementers-guide.md)
- created placeholder README.md files in [`/evm`](/evm) and [`/pallets`](/pallets)

### Clarifications

**General**

- explained the technical differences between each standard
- expanded on double-spending issues and batching recommendations in the [implementers guide](/extrinsics/implementers-guide.md) for the extrinsics-based standards

**1.0.0**

- new examples throughout
- more explicit about optional metadata in [NFTs](/extrinsics/rmrk1.0.0/entities/nft.md) and [Collections](/extrinsics/rmrk1.0.0/entities/collection.md)
- use `balances.transfer` _only_ to facilitate [BUY](/extrinsics/rmrk1.0.0/interactions/buy.md) interactions
- [LIST](/extrinsics/rmrk1.0.0/interactions/list.md) interactions need to exist for at least 5 blocks before the next
- [CONSUME](/extrinsics/rmrk1.0.0/interactions/consume.md) reason optional, defaults to `true`

**2.0.0**
