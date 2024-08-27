# Installation

First of all, make sure you have a working and synced Erigon instance. Setting up Erigon is out of scope of this book.

Otterscan RPC support is already embedded in every Erigon node >= `v2.29.0`. You just need to enable it.

- [Enabling Otterscan support in Erigon](./erigon.md)

This software is currently distributed as a docker image.

- [Install Otterscan from Docker Hub](./dockerhub.md)

Otterscan is a *merged explorer*, which means it can optionally display beacon chain data for chains that support it.

- [Enabling beacon chain support](./beacon-chain/)

Otterscan v2.x has some *EXPERIMENTAL* extra indexers.

- [Enable OTS2 experimental indexers](./ots2.md)

Once you finish installing it, you can check if everything is working as expected.

- [Validating the installation](./validating.md)
