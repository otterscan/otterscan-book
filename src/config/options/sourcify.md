# Alternative Sourcify sources

Otterscan uses smart contract metadata to decode function parameters, name smart contract addresses, and show sources. By default, this data comes from the official Sourcify servers. We encourage you to [self-host this Sourcify data](../../contract-verification/lightweight-sourcify-server.md) to prevent your browsing activity from being leaked to Sourcify, increase the loading speed of this data, ensure continued availability of the data, and reduce the load on Sourcify servers.

Rather than using `repo.sourcify.dev`, you may specify your own Sourcify root URLs in the configuration file by adding the `sourcify` key and adding objects to `sources`:

```json
"sourcify": {
  "sources": {
    "Local Repository": {
      "url": "http://localhost:7077",
      "backendFormat": "RepositoryV2"
    },
    "Sourcify Servers": {
      "url": "https://repo.sourcify.dev",
      "backendFormat": "RepositoryV1"
    }
  }
}
```

The first listed Sourcify source will be the default.

This is useful if you have your own Sourcify repository hosted locally, in which case all of your Otterscan activity will be private. See our section on hosting a [lightweight self-hosted repo](../../contract-verification/lightweight-sourcify-server.md) to download and host a current snapshot of all Sourcify's contracts locally.

The `backendFormat` key current accepts two options:

* `RepositoryV1`
  * All sources are accessible from `/<chainID>/<address>/sources/<source path>`.
* `RepositoryV2`
  * All sources are accessible from `/<chainID>/<address>/sources/<keccak256(source path)>`.
