# EMOTE

React to an [NFT](../entities/nft.md) with an emoticon.

You can only EMOTE on an existing NFT (i.e. one that has not been issued a [CONSUME](consume.md) interaction yet).

## Standard

The format of an EMOTE interaction is:

`0x{bytes(RMRK::EMOTE::{version}::{id}::{emotion})}`

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [NFT](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields)
- `emotion` is the unicode of the emoji (e.g Unicode for party popper 🎉 is
  [1f389](https://emojipedia.org/emoji/🎉/)

### Undo Emote

A user can undo their emote by sending the same emote again. Thus, a [consolidator](https://github.com/rmrk-team/rmrk-tools):

- should consider an emote of type E on NFT N by user U to be absent if `n(U:E,N)%2 == 0`
- should consider an emote of type E on NFT N by user U to be present if `n(U:E,N)%2 != 0`

In other words, if there is an even number of emotes on an NFT by a single user, that user has NOT
emoted. If there is an odd number, that user HAS emoted.

Example:

- Alice gives thumbs up to `11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039`. Now there is one
  👍 emote on this NFT, shown as 👍 from Alice.
- Alice changes her mind, removes thumbs up by sending it again. Now there are 2(👍) on this NFT, no
  👍 is shown on the NFT from Alice.
- Alice talks to artist, she's okay with a thumbs up again, re-sends it. Now there are 3(👍) on this
  NFT, shown as just 👍 from Alice.

### Multiple Emotes

It is perfectly valid for the same user to send multiple emotes to the same NFT. The same rules
about undoing emotes applies individually to each emote.

### Supported Emotes

There is no limitation to which emotes can be sent - any unicode works. However, it is up to the
individual UIs to decide which ones to show. For example, one UI might be so positivity-oriented
they decide to never show 💩. Others might display some emoji which only exist on certain operating
systems. We leave the choice to the implementers and direct them to emojipedia's stats for most used
emoji: https://emojipedia.org/stats/

Emote spam is generally not a concern because each emote costs a transaction fee.

## Examples

To emote 🎉 on `11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039`, the remark would be:

```
RMRK::EMOTE::1.0.0::11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039::1f389
```

And would be submitted as:

```
0x524d524b3a3a454d4f54453a3a312e302e303a3a31313233383333312d6530623962646363343536613336343937612d524d524b424e4e52532d4348554e4b592d303030303030303030303030303033393a3a3166333839
```
