# Assets Server

Otterscan can make use of several external data sources to give context to on-chain data. The external data sources are served to Otterscan clients at the location given by the `assetsURLPrefix` config option.  Note: Do not set `assetURLPrefix` to `/assets` or it will conflict with Otterscan's base assets path.

Typically, these assets are served using the `otterscan/otterscan-assets` Docker image from the [otterscan-assets](https://github.com/otterscan/otterscan-assets) repository. In a development environment, you can use `pnpm run assets-start` and `pnpm run assets-stop` to run the Docker commands.

Setups requiring custom or private data can serve assets manually following the same structure.

## Structure Overview

The asset server serves files in the following folder structure:

* **assets**: Contains token logos from <https://github.com/trustwallet/assets>.
  * `[chain_id]`: Each subdirectory corresponds to a specific blockchain network, such as Ethereum (`1`).
* **chains**: Chain data from <https://github.com/ethereum-lists/chains>.
  * `eip155-[chain_id].json`
* **signatures**: A directory mapping function selectors to function signatures, in the format from <https://github.com/ethereum-lists/4bytes>.
  * Each filename is a 4-byte selector (e.g., `a9059cbb`), and the file contents are the corresponding function signature (e.g., `transfer(address,uint256)`).
* **topic0**: A directory mapping log topic hashes to event signatures, in the format from <https://github.com/otterscan/topic0>.
  * Each filename is a 32-byte hash (e.g., `ddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef`), and the file contents are the corresponding event signature (e.g., `Transfer(address indexed from,address indexed to,uint256 value)`).

## Manually recreating the assets folder structure

You can recreate the assets folder structure and serve it manually.

First, create a new directory and clone all the data repositories into it:
```bash
mkdir external-repos
cd external-repos

git clone https://github.com/trustwallet/assets.git trustwallet-assets
git clone https://github.com/ethereum-lists/chains.git chains
git clone https://github.com/ethereum-lists/4bytes.git 4bytes
git clone https://github.com/otterscan/topic0.git topic0
cd ..
```

Create the main `assets-server` directory:

```bash
mkdir assets-contents
cd assets-contents
```

Set up symbolic links pointing to external data from previously cloned repositories:

```bash
ln -s ../external-repos/chains/_data/chains chains
ln -s ../external-repos/4bytes/signatures signatures
ln -s ../external-repos/topic0/with_parameter_names topic0
```

Set up a symbolic link pointing to the token logos (replace `ethereum` with the chain name):
```bash
mkdir assets
ln -s ../external-repos/trustwallet-assets/blockchains/ethereum/assets ./assets/1
```

(Optional.) Populate a `contracts` folder with the Sourcify repository structure and reuse the assets server as a custom [Sourcify source](../contract-verification/self-hosted-sourcify/setup.md#otterscan-configuration).

### Spin up HTTP server

In a production environment, use a real HTTP server application, like how the `otterscan-assets` image uses `nginx`.

In a development environment, you can use the built-in, single-threaded `http.server` Python module to serve the current directory:

```bash
python3 -m http.server 5175
```

The assets server will be hosted at <http://localhost:5175>.
