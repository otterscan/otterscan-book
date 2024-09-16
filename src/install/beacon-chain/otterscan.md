# Enabling in Otterscan

Add the `BEACON_API_URL` environment parameter to your `docker run` command. The URL must point to an exposed CL REST endpoint which is reachable from your browser.

### Example:

```sh
docker run \
  --rm \
  --name otterscan \
  -d \
  -p 5100:80 \
  --env ERIGON_URL="http://my-erigon-node:8545" \
  --env BEACON_API_URL="http://my-lighthouse-node:5052" \
  otterscan/otterscan:<tag>
```
