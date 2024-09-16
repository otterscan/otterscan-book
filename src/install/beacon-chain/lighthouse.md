# Lighthouse

As of Lighthouse `3.3.0`, the required parameters to make it work with Otterscan are:

```sh
lighthouse beacon \
  --http \
  --http-address "0.0.0.0" \
  --http-allow-origin '*' \
  [<other CLI arguments>]
```
