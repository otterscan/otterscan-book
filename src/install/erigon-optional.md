# Optional flags

## `--ots.search.max.pagesize <N>`

By default the `ots_searchTransactions*` JSON-RPC methods are capped at server level to max page size of 25, no matter what page size is specified in the method parameters.

That is intentional, in order to prevent DoS'ing the node if users were able to specify unbounded page sizes.

In you are using the Otterscan API directly, you may want to increase the maximum allowed page size by setting this parameter on Erigon startup.
