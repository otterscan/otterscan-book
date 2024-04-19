# Caplin

> 💡 Caplin is being actively developed and this document may contain outdated instructions. Please check the Erigon docs if you think that's the case.

Make sure to activate all the following `erigon` executable arguments:

- `--internalcl`: activate internal CL support
- `--caplin.archive`: activate Caplin archive node support
- `--caplin.backfilling`, `--caplin.backfilling.blob`, `--caplin.backfilling.blob.no-pruning`: enable backfilling of historical CL blocks
- `--beacon.api "beacon,builder,config,debug,events,node,validator,lighthouse"`: enable beacon chain REST API namespaces
- `--beacon.api.addr "0.0.0.0"`: expose the REST API endpoint (default is `localhost`)
- `--beacon.api.cors.allow-methods "GET,OPTIONS"`, `--beacon.api.cors.allow-origins '*'`: enable CORS for the REST API endpoint
