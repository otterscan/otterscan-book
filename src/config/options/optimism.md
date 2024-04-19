# Optimism

For chains which follow the OP Stack, configure the `opChainSettings` key in the config:

- `l1ExplorerURL`: The root URL of a block explorer for the layer-1 of this chain, without a trailing forward slash. This appears on block pages in the "L1 Epoch" row.

Example for Sepolia:
```json
{
  "opChainSettings": {
    "l1ExplorerURL": "https://sepolia.otterscan.io"
  }
}
```
