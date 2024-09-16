# Caplin

> 💡 Caplin is being actively developed, and this document may contain outdated instructions. Please check the Erigon docs if you think that's the case.

Make sure to activate all of the following `erigon` arguments:

- `--internalcl`: activate internal CL support (**Erigon 2**)
  > 💡 This flag is not necessary on Erigon 3 since internal CL is enabled by default
- `--caplin.archive`: activate Caplin archive node support
- `--caplin.backfilling`, `--caplin.backfilling.blob`, `--caplin.backfilling.blob.no-pruning`: enable backfilling of historical CL blocks
- `--beacon.api "beacon,builder,config,debug,events,node,validator,lighthouse"`: enable beacon chain REST API namespaces
- `--beacon.api.addr "0.0.0.0"`: expose the REST API endpoint as necessary (default is `localhost`)
- `--beacon.api.port <port-number>`: change the port number for the REST API endpoint (*optional*; default is `5555`)
- `--beacon.api.cors.allow-methods "GET,OPTIONS"`, `--beacon.api.cors.allow-origins '*'`: enable CORS for the REST API endpoint
