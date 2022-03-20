# RMRK.app Extrinsic-based Specifications and Standards

This directory hosts extrinsic-based standards for creating and interacting with [RMRK.app](https://rmrk.app) NFTs.

There are two types of extrinsic-based standards: _entities_ and _interactions_. Entities are definitions for how a
collection, NFT, BASE, or metadata should be declared while being created, so that tools can recognize them as valid.
Interactions are instructions to users for how to prepare remarks to be submitted on chain to maniputate the aforementioned entities.

## Interactions

### The 1.0.0 standard has the following interactions:

- [MINT](rmrk1.0.0/interactions/mint.md) (Minting a collection)
- [CHANGEISSUER](rmrk1.0.0/interactions/changeissuer.md) (Changing the issuer of a collection)
- [MINTNFT](rmrk1.0.0/interactions/mintnft.md) (Minting an NFT inside a collection)
- [SEND](rmrk1.0.0/interactions/send.md) (Sending an NFT to a recipient)
- [LIST](rmrk1.0.0/interactions/list.md) (List an NFT for sale)
- [BUY](rmrk1.0.0/interactions/buy.md) (Buy an NFT)
- [CONSUME](rmrk1.0.0/interactions/consume.md) (Burn an NFT)
- [EMOTE](rmrk1.0.0/interactions/emote.md) (Send a reaction/emoticon)

### The 2.0.0 standard has the following interactions:

- [ACCEPT](rmrk2.0.0/interactions/accept.md) (Accept the addition of a new resource to an existing NFT, or
the addition of a child into a parent NFT)
- [BASE](rmrk2.0.0/interactions/base.md) (Create a Base)
- [BUY](rmrk2.0.0/interactions/buy.md) (Buy an NFT)
- [CHANGEISSUER](rmrk2.0.0/interactions/changeissuer.md) (Changing the issuer of a collection or base)
- [BURN](rmrk2.0.0/interactions/burn.md) (Burn an NFT)
- [CREATE](rmrk2.0.0/interactions/create.md) (Minting a collection, previously MINT)
- [EMOTE](rmrk2.0.0/interactions/emote.md) (Send a reaction/emoticon)
- [EQUIP](rmrk2.0.0/interactions/equip.md) (Equip a child NFT into a parent's slot, or unequip)
- [EQUIPPABLE](rmrk2.0.0/interactions/equippable.md) (Changes the list of equippable collections on a
base's part)
- [LIST](rmrk2.0.0/interactions/list.md) (List an NFT for sale)
- [LOCK](rmrk2.0.0/interactions/lock.md) (Locking a collection)
- [MINT](rmrk2.0.0/interactions/mint.md) (Minting an NFT inside a collection, previously "MINTNFT")
- [RESADD](rmrk2.0.0/interactions/resadd.md) (Add a new resource to an NFT as the collection issuer)
- [SEND](rmrk2.0.0/interactions/send.md) (Sending an NFT to a recipient)
- [SETPROPERTY](rmrk2.0.0/interactions/setproperty.md) (Set a custom value on an NFT)
- [SETPRIORITY](rmrk2.0.0/interactions/setpriority.md) (Set a different order of resource priority)
- [THEMEADD](rmrk2.0.0/interactions/themeadd.md) (Add a new theme to a base)

## Entities

### The 1.0.0 standard has the following entities:

- [COLLECTION + Metadata](rmrk1.0.0/entities/collection.md)
- [NFT + Metadata](rmrk1.0.0/entities/nft.md)

### The 2.0.0 standard has the following entities:

- [Collection](rmrk2.0.0/entities/collection.md)
- [NFT](rmrk2.0.0/entities/nft.md)
- [BASE](rmrk2.0.0/entities/base.md)
- [Metadata](rmrk2.0.0/entities/metadata.md)

## Releases

RMRK standards versions will be tagged as a [release](/rmrk-team/rmrk-spec/releases). Each release will
contain all previous major and minor versions so that an implementer checking out the latest release
can decide which ones to support alongside the latest.

A released version is **never** worked on again - all changes must happen via [RIPs](#contributing)
and will apply to the next version only. It is up to the implementing library / UI to see previous
versions as invalid or demand a migration from an old version to a new version of an NFT (this will
usually involve a [CONSUME](#interactions) of an earlier NFT and a [MINTNFT](#interactions) into a
newer version of the collection) or a [MIGRATE](#interactions) interaction. To be kept up to date on
version changes, please _Watch_ this repo or [subscribe to Dot Leap](https://dotleap.substack.com)
or [NFT Review](https://news.nft.review).

### Standards under development:

- [EVM](../evm)
- [Pallets](../pallets)

## Contributing

To contribute, please create a new RIP (RMRK Improvement Proposal) as per
[this template](https://github.com/Swader/rmrk-spec/issues/new?assignees=&labels=RIP&template=rip.md&title=RIP-XXX+%28please+change+XXX+to+number%29).

## Implementations

Interested in implementing these standards? Check out the
[Implementer's Guide](implementers-guide.md).

The following is a list of implementations of these standards:

- [Singular 1.0](https://singular.rmrk.app/)
- [Kanaria App](https://kanaria.rmrk.app/)
- [Singular 2.0](https://singular.app/)
- [Kodadot](https://kodadot.xyz/)
- [Talisman](https://app.talisman.xyz/nfts)

## Extending the Standard

The RMRK standards will always support the very minimum of functionality needed to interact with
NFTs. However, some projects may want or need more logic, or more specific integration. Let's say
for example that someone is building a CryptoKitties game in which cats can "breed" to produce an
offspring with some random on-chain attributes adding special uniqueness to the new cat token. The
game devs would:

- integrate with an [implementation](#implementations) of RMRK
- wrap RMRK functionality in their own library
- define their own game's rules
- MINT and do other [interactions](#interactions) as per RMRK standards, but reacting to their
  custom rules

As an example, let's assume that we have a user who owns Cat A and Cat B and wants to breed them to
get Cat C. Let's assume that breeding requires 100 blocks of time, and that a breeding results in an
egg, this egg needs to incubate for 100 blocks, and then the user needs to manually hatch it.

To implement this, the CryptoKitties game devs would:

- add a BREED and a HATCH interaction on top of the RMRK standards they already inherit through an
  [implementation](#implementations).
- once BREED is called by a user, the starting block is logged (e.g. X).
- at block X+100, the user can call HATCH and the egg hatches. The block hash is used as a seed for
  the random attributes of the new NFT.
- a new NFT is created and assigned to the breeder.
- given that the NFT is minted according to RMRK standards, it will be automatically compatible with
  any tools and UIs that follow the standard as well.

> Note: A tricky part is step 4 - minting and assigning a new NFT to the breeder. This would require
> a new entity type given that only an issuer is allowed to mint new NFTs, but then the new entity
> is not automatically compatible with wallets and UIs following RMRK, given that they're not
> familiar with the CryptoKitties standard. This introduces a barrier to market entry for the
> CryptoKitties devs. A better alternative is skipping user-initiated hatching and hatching
> programmatically from CryptoKitties game devs' end, but this introduces a reliance on their
> service for the continuation of the game.

> This is an active area of research which we hope to have ironed out soon - ideally with an
> action-triggered MINT addition to the standard.

## Other Information

- [The definitive primer on NFTs](https://bitfalls.com/nft)
- [The RMRK.app website](https://rmrk.app)
