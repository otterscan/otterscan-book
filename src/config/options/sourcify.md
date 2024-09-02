# Alternative Sourcify sources

Rather than using the default `ipfs.io` and `repo.sourcify.dev` sites for accessing Sourcify, you may specify your own Sourcify root URLs in the configuration file by adding the `sourcifySources` key and changing the URLs accordingly:

```json
"sourcifySources": {
  "ipfs": "https://ipfs.io/ipns/repo.sourcify.dev",
  "central_server": "https://repo.sourcify.dev"
}
```

This is useful if you have your own Sourcify repository hosted locally, in which case all of your Otterscan activity will be private.
