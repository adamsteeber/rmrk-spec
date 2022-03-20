# NFT

An NFT is a part of a collection. It is a unique digital asset distinguished by its
`minting block number`,
`collection id`,
`instance id`,
`serial number`, and
`ledger of on-chain interactions`.

NFTs are immutable after creation.

## NFT Standard

An NFT **MUST** adhere to the following standard:

```json
{
  "collection": {
    "type": "string",
    "description": "Collection ID, e.g. e0b9bdcc456a36497a-RMRKBNNRS"
  },
  "name": {
    "type": "string",
    "description": "Name of the NFT. E.g. Chunky, Breach."
  },
  "instance": {
    "type": "string",
    "description": "Instance ID, may be NFT specific or used to create sub categories within the collection. e.g. CHUNKY, BREACH. Must be limited to alphanumeric characters. Underscore is allowed as word separator. E.g. KANARIA-POTION is NOT allowed. KANARIA_POTION is allowed."
  },
  "transferable": {
    "type": "number",
    "description": "If 1, item is transferable. If 0, item is not transferable (i.e. reputation token). If anything else, item will be transferable from that BLOCK onward, e.g. 11400000 means item can be traded after block 11400000."
  },
  "sn": {
    "type": "string",
    "description": "Serial number or issuance number of the NFT, padded so that its total length is 16, e.g. 0000000000000123"
  },
  "metadata?": {
    "type": "string",
    "description": "HTTP(s) or IPFS URI. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "data?": {
    "type": "object",
    "description": "See Data"
  }
}
```

When either metadata or [data](#data) is present, the other is optional. Data takes precedence
always. Note that because metadata contains the description, attributes, third party URLs, etc. it is
still recommended to include it alongside `data`.

### Computed fields

Computed fields are fields that are used in interactions, but are not explicitly set on their
entities. Computed fields are the result of applying a standardized calculation or merger formula to
specific fields. The NFT entity has the following computed fields, to be provided by
implementations:

```json
{
  "id": {
    "type": "computed",
    "description": "An NFT is uniquely identified by the combination of its minting block number, collection ID, its instance ID, and its serial number, e.g. 5193445-0aff6865bed3a66b-ZOMB-ZOMBBLUE-0000000000000001."
  }
}
```

Example id: `11238331-e0b9bdcc456a36497a-RMRKBNNRS-CHUNKY-0000000000000039`.

When processing NFTs and their interactions, tools **MUST** explode the NFT by `-` and if the number of
fragments is anything other than 5, the remark should be discarded as invalid:

- element 0 is the block in which the NFT was minted.
- elements 1 and 2 together make the [collection ID](collection.md).
- element 3 is the instance ID of the NFT (its symbol).
- element 4 is the serial number of the current NFT instance.

### Data

The `data` object is composed of:

- protocol (strict, see Protocols below)
- data
- type (mime type)

#### Protocols

| Protocol  | Mime default           | Description                                                                                                                                    |
| --------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `ipfs`    | image/png              | Points to a directly interpretable resource, be it audio, video, code, or something else                                                       |
| `http(s)` | image/html             | Points to a directly interpretable resource, be it audio, video, code, or something else (not recommended for use)                             |
| `p5`      | application/javascript | Processing.js code                                                                                                                             |
| `js`      | application/javascript | Plain JS code                                                                                                                                  |
| `html`    | text/html              | HTML code, no need for `<html>` and `<body>`, can support dependencies but it's up to the author to prevent the dependencies from disappearing |
| `svg`     | image/svg+xml          | SVG image data                                                                                                                                 |
| `bin`     | n/a                    | binary, directly interpretable                                                                                                                 |

#### Examples

#### A binary video

```json
data: {
  "protocol": "bin",
  "data": "AAAAIGZ0eXBpc29tAAACAGlzb21pc28yYXZjMW1wNDEAAAAIZnJlZQAQC0ttZGF0AQIUGRQmM...",
  "type": "video/mp4"
}
```

#### JavaScript

```json
data: {
  "protocol": "js",
  "data": "alert(\"Hello World\")",
  "type": "application/javascript"
}
```

The above, when interpreted, should immediately pop up a "Hello World" message.

#### NOTE:

- Implementers SHOULD prevent auto-render and auto-play of non-media resources, and in some cases of
  media resources, to protect end users from cross site scripting, crash attacks (malicious NFTs),
  and other nefarious actors.
- Implementers SHOULD expect the mime type to be there, but if missing, default to one common for
  the protocol, e.g. use `application/javascript` is mime type is missing but protocol is `js`.

## Metadata Standard

An NFT **SHOULD** have metadata to describe it and help visualization on various platforms:

```json
{
  "name": {
    "type": "string",
    "description": "Name of the NFT instance. If NAME is present in on-chain, it takes precedence over this one."
  },
  "description": {
    "type": "string",
    "description": "Description of the collection as a whole. Markdown is supported."
  },
  "attributes": {
    "type": "array",
    "description": "An Array of JSON objects, matching OpenSea's: https://docs.opensea.io/docs/metadata-standards#section-attributes"
  },
  "image": {
    "type": "string",
    "description": "HTTP(s) or IPFS URL to project's main image, in the vein of og:image. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "image_data": {
    "type": "string?",
    "description": "[OPTIONAL] Use only if you don't have the image field (they are mutually exclusive and image takes precedence). Raw base64 or SVG data for the image. If SVG, MUST start with <svg, if base64, MUST start with base64:"
  },
  "external_url": {
    "type": "string?",
    "description": "[OPTIONAL] HTTP(s) or IPFS URL for finding out more about this project. If IPFS, MUST be in the format of ipfs://ipfs/HASH"
  },
  "background_color": {
    "type": "string?",
    "description": "[OPTIONAL] Background color of the item. Must be a six-character hexadecimal without #."
  },
  "animation_url": {
    "type": "string?",
    "description": "[OPTIONAL] HTTP(s) or IPFS URL (format MUST be ipfs://ipfs/HASH) for an animated image of the item. GLTF, GLB, WEBM, MP4, M4V, and OGG are supported, and when using IPFS type MUST be appended, separated by colon, e.g. ipfs://ipfs/SOMEHASH:webm."
  },
  "youtube_url": {
    "type": "string?",
    "description": "[OPTIONAL] URL to Youtube video about this NFT"
  }
}
```

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

Metadata:

```json
{
  "name": "Chunky",
  "description": "Chunks in chunks. The avatar flood cometh. Can you find the easter egg?",
  "attributes": [],
  "image": "ipfs://ipfs/bafybeihx3xtigkp7al3gruyg2iypchvrr5j76oryxjprvh5qv32g2bdyt4"
}
```

To see how this NFT was minted, please see the [MINTNFT examples](../interactions/mintnft.md#examples).

----------------

[Kanaria RMRK Banner NFT](https://singular.rmrk.app/collectibles/11238345-e0b9bdcc456a36497a-RMRKBNNRS-BREACH-0000000000000030):

```json
{
  "collection": "e0b9bdcc456a36497a-RMRKBNNRS",
  "name": "Breach",
  "instance": "BREACH"
  "transferable": 1,
  "sn": "0000000000000030",
  "metadata": "ipfs://ipfs/bafkreic4fhsycklvbsqgxzoe5udxtempcfwfbauxbgficuv6txq4dndv4e"
}
```

Metadata:

```json
{
  "name": "Breach",
  "description": "Once more unto the breach, point and click style!",
  "attributes": [],
  "image": "ipfs://ipfs/bafybeibt4nndhhwaqkpsdek4xvlfkqq5tizkank7h6ca6mpbt7d4evvt3i"
}
```

----------------

[Ticket to The Sub Club Premium Sandwich NFT](https://singular.rmrk.app/collectibles/11011587-5860ef1046345507-HOAGIE-PREMIUM-0000000000000001):

```json
{
  "collection": "5860ef1046345507-HOAGIE",
  "name": "Premium",
  "instance": "PREMIUM"
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/bafybeigr7o3ewbsdsvvs26bl5nnyv7cta3jnk2kylcd7nsefpz3fol5p5m"
}
```

Metadata:

```json
{
  "name":"Premium",
  "attributes": [
    {
      "trait_type": "Plate",
      "value": "Uranium Ware"
    },
    {
      "trait_type": "Ingredient Slots",
      "value": 8
    },
    {
      "trait_type": "Base Ingredients",
      "value": 4
    },
    {
      "trait_type": "Rare Ingredients",
      "value": 2
    },
    {
      "trait_type": "Epic Ingredients",
      "value": 1
    },
    {
      "trait_type": "Legendary Ingredients",
      "value": 1
    },
    {
      "trait_type": "Pick Slot",
      "value": "Yes"
    },
    {
      "trait_type": "Pick NFT",
      "value": "Epic"
    },
    {
      "trait_type": "Side Slot",
      "value": "Yes"
    }
  ],
  "description": "Owner of this 1.0 NFT at the time of launch of 2.0 will be rewarded a full sandwich with the following traits:\n\n

    | **Trait**          | **Details**                            |\n
    |:-------------------|:---------------------------------------|\n
    | Plate              | Uranium Ware                           |\n
    | Ingredient Slots   | 8                                      |\n
    | Ingredient NFTs    | 4 Base + 2 Rare + 1 Epic +1 Legendary* |\n
    | Sandwich Pick NFT  | Epic Pick                              |\n
    | Side Slot          | 1 Side Slot                            |\n\n

    Tickets must be burned by the end of 2022 to be rewarded a full 2.0 sandwich.\n\n

    *Legendary ingredient is designed by the NFT owner",
  "image": "ipfs://ipfs/bafybeifpyytl6i56th6oksvkucklouridcwc6f574y33imf3f5kuicijiu",
  "external_url": "http://www.thesubclub.io/"
}
```
