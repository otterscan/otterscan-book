# Install and running Otterscan

## Run Otterscan Docker image from Docker Hub

The official Otterscan repo on Docker Hub is [here](https://hub.docker.com/r/otterscan/otterscan).

There is an image tag for each release tag on GitHub, e.g., `v2.5.0`, `v2.6.0`. In addition to that, there is a `develop` tag which is built automatically from the `develop` branch on GitHub in case you want to run the most recent develop build.

### Example

```
docker run --rm -p 5100:80 --name otterscan -d otterscan/otterscan:<versiontag>
```

This will download the Otterscan image from Docker Hub, run it locally under the `otterscan` container name using the default parameters, and bind it to port 5100 (see the `-p` docker run parameter).

To stop the Otterscan service, run:

```
docker stop otterscan
```

By default, it assumes your Erigon node is at `http://127.0.0.1:8545`, and users' browsers will try to connect to this RPC URL. You can override the URL by setting the `ERIGON_URL` env variable in the container:

```
docker run --rm -p 5100:80 --name otterscan -d --env ERIGON_URL="<your-erigon-node-url>" otterscan/otterscan:<versiontag>
```

You can override the entire Otterscan configuration with the `OTTERSCAN_CONFIG` env variable:

```sh
docker run \
  --rm \
  -p 5100:80 \
  --name otterscan \
  -d \
  --env OTTERSCAN_CONFIG='{
    "erigonURL": "http://127.0.0.1:8545",
    "assetsURLPrefix": "http://127.0.0.1:5175",
    "branding": {
        "siteName": "My Otterscan",
        "networkTitle": "Dev Network"
    },
}' \
otterscan/otterscan:latest
```

These settings overwrite the Otterscan config file on container startup. To disable this behavior, pass `--env DISABLE_CONFIG_OVERWRITE=1` to the Docker command.

This is the preferred way to run Otterscan. You can read about other ways [here](./other.md).

## Run Otterscan development image from Docker Hub

The `develop` branch is automatically built and published on Docker Hub.

There is a helper script that always pulls the latest build and sets the required parameters.

From the repository root:

```
./scripts/run-ots-develop.sh <ERIGON-RPC-URL> <CL-REST-API-URL>
```

It'll start a container under the name `otterscan`.
