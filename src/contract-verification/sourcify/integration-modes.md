# Integration modes

Otterscan supports multiple ways to consume Sourcify data, each one with pros and cons:

## Direct HTTP connection to Sourcify's repository

Standard HTTP connection to the official Sourcify repository at <https://repo.sourcify.dev/>

Fast access to freshly verified data. On the other hand, it is centralized and leaks all addresses you visit in Otterscan to the Sourcify server.

> 💡 This is the default method Otterscan uses out of the box.

## IPFS

We resolve the public Sourcify IPNS to get the latest known IPFS root hash of their repository.

The downside is that recently verified contracts may not have been added yet to the root hash and republished into IPNS.

It uses the public gateway at <https://ipfs.io> by default.

Please see our [ipfs integration docs](./ipfs.md) for more info about how we handle all IPFS integrations and privacy concerns.

## Local snapshot *(deprecated; soon to be removed)*

> ☠️ This method is deprecated and will be removed in the future. The repository is outdated and kept only for historical reasons.

As a midterm solution, we are making available a snapshot docker image of their repository, containing only mainnet full verified contracts.

This would allow you to play with existing contracts up to the snapshot date/time locally, not depending on their service or IPFS connectivity availability.

The Sourcify snapshot is provided as a nginx image at: <https://hub.docker.com/repository/docker/otterscan/sourcify-snapshot>

You can run it with:

```shell
docker run --rm -d -p 3006:80 --name sourcify-snapshot otterscan/sourcify-snapshot:2021-09
```

Stop it with:

```
docker stop sourcify-snapshot
```
