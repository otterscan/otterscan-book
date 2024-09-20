# Lighthouse

As of Lighthouse `3.3.0`, the required parameters to make it work with Otterscan are:

```sh
lighthouse beacon \
  --http \
  --http-address "0.0.0.0" \
  [--http-port <REST-API-port-number> \]
  --http-allow-origin '*' \
  --genesis-backfill \
  [--disable-backfill-rate-limiting \]
  [<other CLI arguments>]
```

- `--http-port` is optional, by default it binds to `:5052`.
- `--genesis-backfill` is required in order to backfill slot data back to the genesis. If you don't specify that parameter and use checkpoint sync, only slot info after the checkpoint will be available.
- `--disable-backfill-rate-limiting` is optional, but if your node is used exclusively for Otterscan you'll probably want to enable it in order to speed up backfilling during the first sync.
