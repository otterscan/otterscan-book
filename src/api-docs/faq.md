# FAQ

## Why don't you use _Some Product XXX_ for Otterscan? And why shouldn't I?

If you are happy using _Some Product XXX_, go ahead.

Otterscan pursues a minimalistic approach and at the same time it is very easy to modify Erigon for your own needs.

Most of the features we implemented are quite basic and it is unfortunate they are not part of the standard API.

> We believe most people end up using _Some Product XXX_ not because of its own unique features, but because the standard JSON-RPC API is quite limited even for basic features.

Implementing everything in-node allows you to plug a dapp directly to your node itself. No need to install any additional indexer middleware or SQL database, each of it own consuming extra disk space and CPU.

> Take Otterscan as an example, **ALL** you need is Otterscan itself (a SPA, can be served by any static provider) and a synced Erigon instance.

## But your API doesn't scale and it is slower than _Some Product XXX_!!!

Not everyone needs to serve thousands of requests per second. Go ahead and use _Some Product XXX_.

Some people just want to run standalone development tools and calculating some data on-the-fly works fine for single user local apps.

Even so, we may introduce custom indexes to speed up some operations in future if there is such demand, so you may opt-in for a better performance by spending more disk space.
