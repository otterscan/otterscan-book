# External data sources

Otterscan makes use of external data not supplied by the blockchain. You can configure these data sources here in order to self-host or disable them.

### Sourcify
Configure your instance's Sourcify sources to specify where contract sources are fetched from.
* [Sourcify](./sourcify.md)

### Solidity compiler downloads
Otterscan's [local contract verification](../../contract-verification/local-contract-verification.md) feature has to download the Solidity compiler in order to recompile the contract sources in your browser and check whether they match the on-chain bytecode. By default, these are downloaded from `https://binaries.soliditylang.org`, but you can change the source in your Otterscan config:

```json
  "externalDataSources": {
    "contractCompilerBaseURL": "https://binaries.soliditylang.org"
  }
```
