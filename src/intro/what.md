# What is it?

Otterscan is a block explorer for EVM chains designed to run locally with an archive node companion—more specifically, Erigon.

This approach brings many advantages, as follows.

## Privacy

You are querying your own node, so you are not sending your IP address or queries to an external, third-party node.

## Fast

Since you are querying your local archive node, everything is fast. No network roundtrips are necessary.

## Actually, very fast

This software was designed to be a companion of Erigon, a blazingly fast archive node.

## Really, it is even faster

The standard web3 JSON-RPC methods are quite verbose and generic requiring many calls to gather many pieces of information at client side.

We've implemented some custom methods at the `rpcdaemon` level, so less information is needed to be JSON-marshalled and transmitted over network.
