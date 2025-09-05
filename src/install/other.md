# Other ways to run Otterscan

## (Alternative 1) Build Otterscan docker image locally and run it

If you don't want to download it from Docker Hub, you can build the docker image from the sources and run it.

If you just want to build the image locally, there is no need to install the development toolchain, just make sure you have a recent working Docker installation.

This method requires only `git` and `docker`.

The entire build process will take place inside the docker multi-stage build.

Clone Otterscan repo.

```
git clone https://github.com/otterscan/otterscan.git
cd otterscan
pnpm docker-build
```

This will run the entire build process inside a build container, merge the production build of the React app with pre-made assets from `otterscan-assets` project into the same image format it is published in Docker Hub, but locally under the name `otterscan`.

Then you can start/stop it using the commands:

```
pnpm docker-start
<browse locally on http://localhost:5273>
pnpm docker-stop
```

## (Alternative 2) Run a development build from the source

First, a brief explanation about the app:

- The app itself is a simple React app which will be run locally and communicates with your Erigon node.
- The app makes use of external data sources, like icons, signatures, etc. They require you to run a separate process to serve them. They are made available as a nginx Docker image.

These instructions are subjected to changes in future for the sake of simplification.

Make sure you have a working node 22/pnpm 10 installation.

By default, it assumes your `erigon` process is serving requests at <http://localhost:8545>, a beacon chain REST API serves requests at <http://localhost:5052>, and assets are hosted at <http://localhost:5175>.

You can customize these URLs by creating a `.env.development.local` file at the root of the repository (automatically git ignored) and adding the following entries:

```
VITE_ERIGON_URL=<your-erigon-JSON-RPC-endpoint>
VITE_BEACON_API_URL=<your-beacon-chain-REST-API-endpoint>
VITE_ASSETS_URL=<your-assets-endpoint>
```

Start serving the assets server by running the following Docker image:

```
pnpm assets-start
```

To stop it, run:

```
pnpm assets-stop
```

By default they run at <http://localhost:5175>

To run Otterscan development build:

```
pnpm i
pnpm start
```

Otterscan should now be running at <http://localhost:5173>
