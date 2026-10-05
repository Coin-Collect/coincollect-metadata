# CoinCollect presentation metadata

This repository is the presentation-only source for the CoinCollect V1 NFT pool catalogue. The frontend consumes the
published document at <https://metadata.coincollect.org/nft-pools.json> and continues to use its on-chain registry and
fallback artwork if this document is absent or unavailable.

## Files

- `nft-pools.json` is the canonical V1 document.
- `schema/nft-pools.schema.json` documents its JSON shape and bounds.
- `assets/pools/<lowercase-pool-address>/banner.webp` and `avatar.webp` hold optional pool artwork.

The document contains **presentation fields only**: name, banner, avatar, project URL, Get NFT URL, description, and
Partner/Community category. The canonical `137:<lowercase-address>` ID associates a presentation record with a pool
that the frontend independently verifies from Polygon factory events. Do not add factory, reward, collection, staking,
schedule, threshold, fee, or other protocol fields. Metadata never changes pool verification, readiness, or writes.

## Updating a pool

1. Copy the one-record example shape from `nft-pools.json` and add a unique `137:0x` plus 40 lowercase hexadecimal
   address ID. Keep IDs unique across the complete document.
2. Add only the supported presentation fields. Use absolute HTTP(S) URLs; for repository artwork, use
   `https://metadata.coincollect.org/assets/pools/<lowercase-address>/banner.webp` and `avatar.webp`.
3. Validate the document against `schema/nft-pools.schema.json`. The frontend also enforces canonical IDs, duplicate-ID
   rejection, safe URL protocols, bounded strings, at most 2,000 records, and a 1 MB document limit.
4. Set `updatedAt` to the current ISO-8601 UTC timestamp and open a pull request to `main`. The Pages workflow publishes
   changes merged to `main`.

The frontend revalidates every five minutes, times out requests after four seconds, and keeps the last valid document for
up to 24 hours during an outage. A bad metadata deployment cannot remove a discovered pool or make a transaction
available.

## Domain and static hosting setup

The data repository is not itself the production host. The domain owner must provision `metadata.coincollect.org`, point
it at the approved static host/CDN, and enable HTTPS. Publish only `nft-pools.json` and `assets/`; schema and operator
documentation are not public catalogue data. After provisioning, verify that the custom URL serves JSON with the
correct content type and an `Access-Control-Allow-Origin` response usable by `https://coincollect.org`. If the host does
not provide that CORS header, configure the CDN to add it before enabling the production consumer.

Do not point the frontend at GitHub's raw-content URL as a fallback. The stable CoinCollect domain is the consumer
contract.
