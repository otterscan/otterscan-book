# Architecture

Otterscan itself can be split into three parts:

- [Otterscan UI](#otterscan-ui)
- [Otterscan API Specification](#otterscan-api-specification)
- [Otterscan API Implementation](#otterscan-api-implementation)

## Otterscan UI

This is the main block explorer UI that end users will interact with.

It is a React single-page application, and the repository is available at <https://github.com/otterscan/otterscan>.

## Otterscan API Specification

This is the set of API definitions the Otterscan UI depends on.

See more [here](../api-docs).

## Otterscan API Implementation

This is server software that implements the Otterscan API.

The reference implementation is [Erigon](https://github.com/erigontech/erigon), but any Ethereum client (in theory) could implement it.
