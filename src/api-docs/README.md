# API Spec

> This chapter only makes sense if you intend to dive deep into Otterscan source code or want to integrate directly with the Otterscan APIs.

## Specification

There is an ongoing effort to formalize the Otterscan API spec in the same format as the standard JSON-RPC APIs.

- Online spec: <https://otterscan.github.io/execution-apis/api-documentation/>
- The repository is at: <https://github.com/otterscan/execution-apis>.

A more detailed explanation of the `ots` namespace:

- [ots namespace spec](./ots-api.md)

There is an experimental `ots2` namespace, which is likely to change, but here is an overview:

- [ots2 namespace spec](./ots2-api.md)

## Design Goals

Otterscan's RPC namespaces aim to provide a minimalistic solution for Ethereum data access which overcomes the limitations of the standard JSON-RPC API. Implementing features directly in-node eliminates the need for additional indexer middleware, databases, or dependencies. Our design prioritizes simplicity and flexibility over high-performance scalability.
