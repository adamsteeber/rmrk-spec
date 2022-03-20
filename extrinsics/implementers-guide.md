# RMRK Implementer's Guide for Extrinsic-based Standards

Because RMRK is append-only and no state is being changed, every new block has the potential to
introduce non-obvious state changes into the global NFT space living on RMRK. As an example, an
[NFT](rmrk2.0.0/entities/nft.md) can change ownership due to a
[SEND](rmrk2.0.0/interactions/send.md) interaction, or due to a
[BUY](rmrk2.0.0/interactions/buy.md) interaction. It is an implementer's duty to reconcile
all these changes across historical blocks to get an accurate picture of the current state.

> Note: By implementing things in the wrong order or not following recommended implementation
> guidelines, an implementer can cause financial loss to NFT minters, buyers, and sellers. There is
> no way to distinguish invalid interaction submissions compatible with RMRK from those that are
> valid, except by reconciling the **whole RMRK history from genesis**.

## Remark Dumps

For convenience, dumps of RMRK remarks (i.e. prefix = "rmrk" or "RMRK") on the Kusama blockchain can be found below:

- [1.0.0](dumps/1.0.0.zip)

- [2.0.0](dumps/2.0.0.zip) 

Running the `fetch` function of [rmrk-tools](https://github.com/rmrk-team/rmrk-tools) with the
appropriate block numbers will produce the same dumps, should one wish to double-check these data
sets.

## Consolidator

A consolidator is a tool that takes remarks from a dump as input, processes them one by one, and comes up with a consolidated state of the NFT ecosystem on
the respective chain. The reference implementation is the [rmrk-tools](https://github.com/rmrk-team/rmrk-tools)
consolidator.

## Syncing

When using a server, this is easy. Load into a database and serve from there. However, to properly sync with the state in a decentralized way, an implementer must:

- grab a previously created dump of remarks until block X
- run a fetch operation to grab remaks from X to last finalized at time of page load (F)
- subscribe to new blocks since the page loaded (Y)

A possible problem occurs in cases where F < Y. Finalized blocks may lag a bit, and by default we only consolidate finalized blocks to be sure a tx went through.

To help implementers get around this problem, a [Listener tool](https://github.com/rmrk-team/rmrk-tools) exists in the official tools repo.
## Edge Cases and Caveats

### Missing version

Spec 0.1 had a bug - the version number of the standard was not included in the MINT and MINTNFT
interactions. Where missing, this version should be implied to mean 0.1. Since 1.0.0 onwards every
interaction has a version number.

### Batching

- `batchAll` should be used when submitting a batch of remarks, however `batch` may also be used but is not recommended. Although Kusama can support 10,000+ transactions in a single batch,
it is highly recommended to submit **at most only 100 RMRK remarks** at a time with **at least 2 minutes** inbetween each batch of 100.

### Double-spending

- It is possible that two BUY interactions from two separate users for the same NFT occur in the same block. This is problematic because both balance transfers will
succeed but only the first BUY interaction in the block will be honored by the consolidator. To prevent this, UIs should disable interactions on their frontend while
their backend is preparing a transaction based on a previous user input. For popular NFTs, it is inevitable that two users will input an intention to BUY on the frontend at the same time.
There is no official recommended solution to this inherent flaw with the extrinsic-based standards except to implement the [evm-based standard](../evm).

### Other special cases

- It is important for implementers to prevent users from issuing multiple BUYs in a single batch. If a user wants to they can of course manually build such transactions,
and the UI should see the first transaction in a block as valid, but UIs should absolutely prevent this.
- it is an implementer's responsibility to accurately track duplicate interactions in a single block. If a single block contains a LIST and a CONSUME, then whichever comes first in the finalized block is valid, the other is discarded. 
The Consolidator from the [official tools](https://github.com/rmrk-team/rmrk-tools) should help with this.

---
