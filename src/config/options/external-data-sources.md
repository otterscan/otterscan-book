# External data sources

Otterscan makes use of external data not supplied by the blockchain. You can configure these data sources here in order to self-host or disable them.

### Sourcify
Configure your instance's Sourcify sources to specify where contract sources are fetched from.
* [Sourcify](./sourcify.md)

### Solidity compiler downloads
Otterscan's [local contract verification](../../contract-verification/local-contract-verification.md) feature has to download the Solidity compiler in order to recompile the contract sources in your browser and check whether they match the on-chain bytecode. By default, these are downloaded from `https://binaries.soliditylang.org`, but you can change the source or disable the feature in your Otterscan config:

```json
  "sourcify": {
    "localContractVerification": {
      "enabled": true,
      "contractCompilerBaseURL": "https://binaries.soliditylang.org"
    }
  }
```

### CCIP reads
By default, Otterscan supports [CCIP reads](https://eips.ethereum.org/EIPS/eip-3668) during ENS lookups and reverse lookups only. CCIP reads fetch data from arbitrary external sources; for example, looking up the ENS name `jesse.cb.id` causes an external data request to `api.coinbase.com` to fetch the corresponding address. CCIP reads can be triggered by reverse lookups, too, and Otterscan makes these over the course of regular browsing to display ENS names. Although this feature is enabled by default, **if you value privacy, we recommend you disable CCIP reads** because they leak your IP address to external parties. Disabling CCIP reads will affect ENS address lookups for certain ENS subdomains.

To disable CCIP reads for ENS lookups, set `ensLookups` to `false` under `externalDataSources`/`ccip` in your Otterscan config:

```json
  "externalDataSources": {
    "ccip": {
      "ensLookups": false
    }
  }
```
