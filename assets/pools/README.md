# Pool artwork

Create one directory per canonical Polygon pool address, using lowercase hexadecimal without a chain prefix:

```text
assets/pools/0x0123456789abcdef0123456789abcdef01234567/
├── banner.webp
└── avatar.webp
```

Reference these files from `nft-pools.json` using the production metadata hostname. Keep images optimized and free of
embedded HTML or scripts; the frontend renders metadata text as plain text and treats image URLs as presentation only.
