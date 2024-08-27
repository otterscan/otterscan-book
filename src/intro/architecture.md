# Architecture

Otterscan itself can be split into 3 parts:

- [Otterscan UI](#otterscan-ui)
- [Otterscan API Specification](#otterscan-api-specification)
- [Otterscan API Implementation](#otterscan-api-implementation)

## Otterscan UI

That is the main block explorer UI that end users will interact with.

It is a React SPA and the repository is available at: <https://github.com/otterscan/otterscan>.

## Otterscan API Specification

It is a set of node-level API definitions Otterscan UI depends on.

See more [here](../api-docs).

## Otterscan API Implementation

It is a server software that implements the Otterscan API.

The reference implementation is [Erigon](https://github.com/erigontech/erigon), but any Ethereum client (in theory) could implement it.
