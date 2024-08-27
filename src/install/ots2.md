# Otterscan 2.x EXPERIMENTAL indexers

Otterscan v2.x supports some experimental, optional indexers.

They require some extra steps to be enabled.

> ⚠️ The following instructions are for Erigon 2.x but their support are deprecated. We are currently working on Erigon 3 support and both new APIs and features in the UI will likely change.

> ⚠️ They require **MORE** disk space and first sync time in order to generate the extra information.

## Clone and build Erigon + OTS2 support

Checkout the `ots2-alpha4` branch from Erigon repository: https://github.com/erigontech/erigon/tree/ots2-alpha4

Build it as usual with `make` command.

## Enable OTS2 indexers inside Erigon

Change Erigon CLI args to:

- Enable `ots2` API namespace in addition to `ots`.
- Add `--experimental.ots2` CLI arg.

For example, if your Erigon start command is:

```shell
erigon \
        --http.api "eth,erigon,ots" \
        [<other CLI arguments>]
```

change it to:

```shell
erigon \
        --http.api "eth,erigon,ots,ots2" \
        --experimental.ots2 \
        [<other CLI arguments>]
```

## Enable OTS2 mode in Otterscan

Add the `OTS2=true` env variable when starting the docker container.

For example, if your docker start command is:

```shell
docker run --rm --name otterscan -d -p 5100:80 --env ERIGON_URL="<erigon-url>" otterscan/otterscan:v2.3.0
```

change it to:

```shell
docker run --rm --name otterscan -d -p 5100:80 --env ERIGON_URL="<erigon-url>" --env OTS2=true otterscan/otterscan:v2.3.0
```
