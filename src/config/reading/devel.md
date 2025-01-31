# Development

During development, define a `VITE_CONFIG_JSON` variable inside a `.env.development.local` file, and vite will use those settings automatically.

Otterscan uses [dotenv](https://github.com/motdotla/dotenv), and that file is automatically `.gitignore`'d and not pushed into version control.

Example `.env.development.local` file:

```
VITE_CONFIG_JSON='
{
  "erigonURL": "http://your-erigon-node-ip:8545",
  "beaconAPI": "http://your-beacon-node-ip:5052",
  "assetsURLPrefix": "http://localhost:5175",
  "experimentalFixedChainId": 11155111,
  "chainInfo": {
    "name": "Sepolia Testnet",
    "faucets": [],
    "nativeCurrency": {
      "name": "Sepolia Ether",
      "symbol": "SEPETH",
      "decimals": 18
    }
  },
  "sourcify": {
    "sources": {
      "Sourcify Servers": {
        "url": "https://repo.sourcify.dev",
        "backendFormat": "RepositoryV1"
      }
    }
  }
}
'
```
