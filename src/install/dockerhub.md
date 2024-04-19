# Install and running Otterscan

## Run Otterscan docker image from Docker Hub

The Otterscan official repo on Docker Hub is [here](https://hub.docker.com/r/otterscan/otterscan).

There is an image tag for each release tag on GitHub, e.g., `v2.5.0`, `v2.6.0`. In addition to that, there is a `develop` tag which is built automatically from the `develop` branch on GitHub in case you want to run the most recent develop build.

### Example

```
docker run --rm -p 5100:80 --name otterscan -d otterscan/otterscan:<versiontag>
```

This will download the Otterscan image from Docker Hub, run it locally under the `otterscan` container name using the default parameters, binding it to port 5100 (see the `-p` docker run parameter).

To stop Otterscan service, run:

```
docker stop otterscan
```

By default it assumes your Erigon node is at <http://127.0.0.1:8545>. You can override the URL by setting the `ERIGON_URL` env variable on docker run:

```
docker run --rm -p 5100:80 --name otterscan -d --env ERIGON_URL="<your-erigon-node-url>" otterscan/otterscan:<versiontag>
```

This is the preferred way to run Otterscan. You can read about other ways [here](./other.md).

## Run Otterscan development image from Docker Hub

The `develop` branch is automatically built and published on Docker Hub.

There is a helper script that always pulls the latest build and sets the required parameters.

From the repository root:

```
./scripts/run-ots-develop.sh <ERIGON-RPC-URL> <CL-REST-API-URL>
```

It'll start a container under the name `otterscan`.
