# Alternative Sourcify sources

Rather than using the default `ipfs.io` and `repo.sourcify.dev` sites for accessing Sourcify, you may specify your own Sourcify root URLs in the configuration file by adding the `sourcify` key and changing the `sources` URLs accordingly:

```json
"sourcify": {
  "sources": {
    "ipfs": "https://ipfs.io/ipns/repo.sourcify.dev",
    "central_server": "https://repo.sourcify.dev"
  },
  "backendFormat": "RepositoryV1"
}
```

This is useful if you have your own Sourcify repository hosted locally, in which case all of your Otterscan activity will be private. See our section on hosting a [lightweight self-hosted repo](../../contract-verification/lightweight-sourcify-server.md) to download and host a current snapshot of all Sourcify's contracts locally.

The `backendFormat` key current accepts two options:

* `RepositoryV1`
  * All sources are saved to `/<chainID>/<address>/sources/<source path>`.
* `RepositoryV2`
  * All sources are saved to `/<chainID>/<address>/sources/<keccak256(source path)>`.
