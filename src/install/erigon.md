# Running Otterscan with Erigon

## Enable the Otterscan namespace on erigon

When running Erigon, make sure to enable the `eth`, `erigon` and `ots` namespaces in addition to whatever CLI options you are using to start Erigon.

Enabling namespaces in Erigon is done through the `--http.api` argument.

### Example

```shell
erigon --http.api "eth,erigon,ots[,<other-namespaces>]" [<other CLI arguments...>]
```

> `ots` stands for Otterscan and it is the JSON-RPC namespace we use for our own custom APIs.

## Enable CORS

CORS is required for the browser to call the Erigon node directly.

This is done using the `--http.corsdomain` argument. Make sure it is set correctly to the domain you are binding your Erigon node to.

> 💡 If you are going to enable CORS for all domains, make sure to quote it, i.e. `--http.corsdomain='*'` otherwise the wildcard (`*`) may be captured by the shell.

## Example

```shell
erigon --http.corsdomain '*' [<other CLI arguments...>]
```
