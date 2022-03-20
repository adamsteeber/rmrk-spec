# RMRK Multi-Chain Specifications

So far there are three fundamental RMRK standards:

## [Extrinsics](/extrinsics)

This is the standard that pioneered RMRK technology and inspired the RMRK name. All
[substrate blockchains](https://github.com/paritytech/substrate/tree/b4c188b3904209aba4b331ab19c46c8d0ff64d3c) have a `system.remark` extrinsic built into their default frames.
A standard for formatting strings to be submitted as remarks on a substrate blockchain has been established to instruct users and implementers on how to encode and manipulate
NFTs using `system.remark` extrinsics.

While being the easiest standard to implement it comes with many inherent flaws that are unacceptable for any NFT technology.

## [EVM](/evm)

Due to the flaws in the extrinsic standard, such as double-spending and append-dependency, RMRK has established a standard of EVM smart contracts written in Solidity. This facilitates true
tokenization of digital assets which allows NFTs to be managed by transparent and trustless smart contracts. The initial launch of this standard was on [Moonriver](https://moonbeam.network/networks/moonriver/),
however, since the smart contracts adhere to EVM, this standard may be implemented on any EVM-based chain.

## [Pallets](/pallets)

Substrate blockchains are written in Rust and are comprised of "pallets" which are merely logical configurations and encoded functionalities. In a sense they are like smart contracts, but
a substrate blockchain's pallets co-exist under the same runtime which allows for a totally independent ecosystem of smart contracts that can be repetedly and simultaneously upgraded.
Unlike the EVM standard, the RMRK pallets standard will allow any _substrate_ blockchain to include RMRK pallets as add-ons in their runtime. This will allow substrate blockchains to implement
RMRK NFTs seemlessly and democratically since runtime upgrades are usually decided via governance referenda.
