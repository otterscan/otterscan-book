# Setup

## Run a local Sourcify instance on your dev network

### Launch a local chain

Launch an Otterscan-compatible local Ethereum RPC node hosting your development server.

- Erigon:
  - ```
    ./erigon \
      --chain=dev \
      --datadir=dev \
      --http.api eth,erigon,trace,ots,ots2 \
      --http.corsdomain "*" \
      --http.vhosts "*" \
      --mine \
      --fakepow
    ```
  - Note: The Erigon devnet only supports a pre-Shanghai (pre-Merge) version of Ethereum. When compiling your contracts, you'll have to set the `evmTarget` to `london` for them to run at all on the devnet.
    - You'll have to create a full-fledged Ethereum test network in Erigon if you want to run a modern version of the EVM.
- Foundry's [Anvil](https://book.getfoundry.sh/reference/anvil/):
  - `anvil`
  - Note: Anvil does not support ots2 RPC methods, so make sure `experimental` is set to `false` in your Otterscan config.

### Sourcify

1. Clone the Sourcify repository:

    ```sh
    git clone https://github.com/ethereum/sourcify.git
    ```

2. To add support for your local blockchain, create a JSON file `services/server/src/sourcify-chains.json`:

    ```json
    {
      "1337": {
        "sourcifyName": "Local chain",
        "supported": true,
        "rpc": [
          "http://localhost:8545"
        ]
      }
    }
    ```

    You can adjust the chain ID `1337` and the RPC host `http://localhost:8545` as needed. The default chain ID for the Erigon devnet is 1337, and the default for Anvil is 31337.

3. Adjust the repository URL in `ui/.env.development` since for simplicity we aren't using Sourcify's h5ai-nginx file viewer:

    ```sh
    REACT_APP_REPOSITORY_SERVER_URL=http://localhost:5555/repository
    ```

4. Build all necessary Sourcify components, notably the server and the UI:

    ```sh
    npm run build:clean
    ```

5. Start the Sourcify server:

    ```sh
    NODE_ENV=development npm run server:start
    ```

6. Start the Sourcify UI:

    ```sh
    npm run ui:start
    ```

### Otterscan configuration

To use the local Sourcify instance, you need to point to it in Otterscan's configuration file.
In your Otterscan configuration JSON, specify your local Sourcify repository as the Sourcify source:

```json
"sourcify": {
  "sources": {
    "ipfs": "http://localhost:5555/repository",
    "central_server": "http://localhost:5555/repository"
  },
  "backendFormat": "RepositoryV1"
}
```
