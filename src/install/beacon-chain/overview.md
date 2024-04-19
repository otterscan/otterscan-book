# Overview

Like EL has *blocks* and *transactions*, CL has its own set of data structures. It is possible to read this information from CL through a standardized [Beacon Chain REST API](https://ethereum.github.io/beacon-APIs/).

## Gotchas

There are many CL implementations that you can combine with your EL, hence many possible issues or different behaviors, even in the same implementation depending on how it is configured.

We have been mainly using/testing both [Lighthouse](https://github.com/sigp/lighthouse/) (in its "experimental tree states") and Erigon's own internal CL "Caplin" since they have checkpoint sync and backfilling.

It is out-of-scope of this document to review all existing CL implementations.

Let's now describe in more details some issues we noticed.

## Checkpoint vs. genesis sync

Checkpoint sync is a nice feature that allows you to "jump" directly to a finalized slot and have a functional CL node in a matter of minutes.

However doing that you won't have the historical information, i.e., you won't be able to explore older slots, epochs, etc.

That may not be such a problem if you are only concerned with your current validator balance, for example. Just be aware of that.

The alternative if you want to browse all beacon chain history is to do a genesis sync, but that is very slow on mainnet.

## Backfilling

Some CL implementations (e.g. [Lighthouse](https://github.com/sigp/lighthouse/)) have a nice feature called *backfilling*, where you can do a checkpoint sync, have a functional CL node in minutes, but it downloads historical information in background.

That may be a better alternative to genesis sync if you want to have all historical info.

## Some historical operations can be slow on default settings

Let's take as an example the `/eth/v1/validator/duties/proposer/` call that we use to obtain the elected block proposers for a given epoch.

Its main use is to answer the question: for a missed slot, who was the elected validator who missed it?

Well, in Lighthouse, using the default settings, querying an old epoch is very slow: <https://github.com/sigp/lighthouse/issues/3770>

The alternative is to change data granularity, trading off disk space for speed.

There may be other cases with different trade-offs. Be aware of how your CL implementation works.
