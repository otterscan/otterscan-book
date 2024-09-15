# ots namespace spec

The [standard Ethereum JSON-RPC APIs](https://ethereum.org/en/developers/docs/apis/json-rpc/) are very limited and in some cases non-performant for what you can do with an archive node.

There is plenty of useful data that can be extracted and we implemented some extra RPC methods for them.

They are all used by Otterscan, but we are documenting them here so others can try it, give feedback and eventually get it merged upstream if they are generalized enough.

We take an incremental approach when design the APIs, so there may be some methods very specific to Otterscan use cases, others that look more generic.

Those APIs are implemented at EL-client level under the `ots` JSON-RPC namespace.

## How do I use it?

They are all JSON-RPC methods, so your favorite web3 library _should_ have some way to custom call them.

For example, [ethers.js](https://github.com/ethers-io/ethers.js) wraps standard calls in nice, user-friendly classes and parses results into easy-to-use objects, but also allows you to do custom calls and get raw results while still taking advantage of their capabilities like automatic batching, network timeout handling, etc.

I'll use ethers.js as an example here because it is what I use in Otterscan. Please check your web3 library docs for custom call support.

### Example

Let's call the `ots_getTransactionError` method to obtain the revert reason of a failed transaction. It accepts one string parameter containing the transaction hash and returns a byte blob that can be ABI-decoded:

```typescript
const provider = ...; // Obtain a JsonRpcProvider object
const txHash = "..."; // Set the transaction hash
const result = (await provider.send("ots_getTransactionError", [txHash])) as string;
```

## Method summary

All methods are prefixed with the `ots_` namespace in order to make it clear it is vendor-specific and there is no name clash with other same-name implementations.

| Name              | Description      | Reasoning |
|-------------------|------------------|-----------|
| `ots_getApiLevel`           | Totally Otterscan internal API, absolutely no reason for anything outside Otterscan to use it. | Used by Otterscan to check if it's connecting to a compatible patched Erigon node and display a friendly message if it is not. |
| `ots_getInternalOperations` | Returns the internal ETH transfers inside a transaction. | For complex contract interactions, there may be internal calls that forward ETH between addresses. A very common example is someone swapping some token for ETH, in which case there is an ETH send to the sender address which is only unveiled by examining the internal calls. |
| `ots_hasCode`               | Check if a certain address contains deployed code. | A common way to check if an address is a contract or an EOA is calling `eth_getCode` to see if it has some code deployed. However this call is expensive data-wise if the contract has a lot of deployed code. This call just returns a boolean. |
| `ots_getTransactionError`   | Extract the transaction's raw error output. | In order to get the error message or custom error from a failed transaction, you need to get its error output and decode it. This info is not exposed through standard APIs. |
| `ots_traceTransaction`      | Extract all variations of calls, contract creations, and self-destructs and return a call tree. | This is an optimized version of tracing; regular tracing returns lots of data, and custom tracing using a JS tracer could be slow. |
| `ots_getBlockDetails`       | Tailor-made and expanded version of `eth_getBlock*` for the block details page in Otterscan. | The standard `eth_getBlock*` is quite verbose and it doesn't bring all info we need. We explicitly remove the transaction list (unnecessary for that page and also this call doesn't scale well), log blooms, and other unnecessary fields. We add issuance and block fees info to the response. |
| `ots_getBlockDetailsByHash` | Same as `ots_getBlockDetails`, but it accepts a block hash as a parameter. | |
| `ots_getBlockTransactions`  | Get paginated transactions for a certain block. Also remove some verbose fields like logs. | As the block size increases, getting all transactions from a block at once doesn't scale, so the first point here is to add pagination support. The second point is that receipts may have big, unnecessary information, like logs. So we cap all of them to save network bandwidth. |
| `ots_searchTransactionsBefore` and `ots_searchTransactionsAfter` | Gets paginated inbound/outbound transaction calls for a certain address. | There is no native support for any kind of transaction search in the standard JSON-RPC API. We don't want to introduce additional indexer middleware in Otterscan, so we implemented an in-node search. |
| `ots_getTransactionBySenderAndNonce` | Gets the transaction hash for a certain sender address, given its nonce. | There is no native support for this search in the standard JSON-RPC API. Otterscan needs it to enable navigation between nonces from the same sender address. |
| `ots_getContractCreator` | Gets the transaction hash and the address which created a contract. | No way to get this info from the standard JSON-RPC API. |

## Method details

> Some methods include a sample call so you call try it from the CLI. The examples use `curl` and assume you are running `rpcdaemon` at `http://127.0.0.1:8545`.

### `ots_getApiLevel`

Very simple API versioning scheme. Every time we add a new capability, the number is incremented. This allows for Otterscan to check if the Erigon node contains all API it needs.

Parameters:

`<none>`

Returns:

- `number` containing the API version.

### `ots_getInternalOperations`

Trace internal ETH transfers, contract creations (CREATE/CREATE2) and self-destructs for a certain transaction.

Parameters:

1. `txhash` - The transaction hash.

Returns:

- `array` of operations, sorted by their occurrence inside the transaction.

The operation is an object with the following fields:

- `type` - transfer (`0`), self-destruct (`1`), create (`2`) or create2 (`3`).
- `from` - the ETH sender, contract creator or contract address being self-destructed.
- `to` - the ETH receiver, newly created contract address or the target ETH receiver of a self-destruct.
- `value` - the amount of ETH transferred.

### `ots_hasCode`

Check if an ETH address contains a deployed code.

Parameters:

1. `address` - The ETH address to be checked.
2. `block` - The block number at which the code presence will be checked or "latest" to check the latest state.

Returns:

- `boolean` indicating if the address contains a bytecode or not.

Example 1: does Uniswap V1 Router address have a code deployed? (yes, it is a contract)

Request:

```sh
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '
  {
    "jsonrpc":"2.0", 
    "id": 1, 
    "method":"ots_hasCode",
    "params":
      [
        "0xc0a47dFe034B400B47bDaD5FecDa2621de6c4d95",
        "latest"
      ]
  }' \
  http://127.0.0.1:8545
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": true
}
```

Example 2: Does Vitalik's public address have code deployed to it? (no, it is an EOA)

Request:

```sh
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '
  {
    "jsonrpc":"2.0",
    "id": 1,
    "method":"ots_hasCode",
    "params":
      [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "latest"
      ]
  }' \
  http://127.0.0.1:8545
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": false
}
```

### `ots_traceTransaction`

Trace a transaction and generate a trace call tree.

Parameters:

1. `txhash` - The transaction hash.

Returns:

- `object` containing the trace tree.

### `ots_getTransactionError`

Given a transaction hash, returns its raw revert reason.

The returned byte blob should be ABI decoded in order to be presented to the user.

For instance, the most common error format is a `string` revert message; in this case, it should be decoded using the `Error(string)` method selector, which will allow you to extract the string message.

If this is not the case, it is probably a Solidity custom error, so you must have the custom error ABI in order to decode it.

Parameters:

1. `txhash` - The transaction hash.

Returns:

- `string` containing the hexadecimal-formatted error blob or simply a "0x" if the transaction was successfully executed. It returns "0x" if it failed with no revert reason or out of gas, so make sure to analyze this return value together with the transaction success/fail result.

Example: get the revert reason of a random Uniswap v3 transaction spotted in the wild.

Request:

```sh
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '
  {
    "jsonrpc":"2.0",
    "id": 1,
    "method":"ots_getTransactionError",
    "params":
      [
        "0xcdb0e53c4f1b5f37ea7f0d2a8428b13a5bff47fb457d11ef9bc85ccdc489635b"
      ]
  }' \
  http://127.0.0.1:8545
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x08c379a0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000135472616e73616374696f6e20746f6f206f6c6400000000000000000000000000"
}
```

> ABI-decoding this byte string against `Error(string)` should result in the "Transaction too old" error message.

### `ots_getBlockDetails`

Given a block number, return its data. Similar to the standard `eth_getBlockByNumber/Hash` method, but optimized.

Parameters:

1. `number` representing the desired block number.

Returns:

- `object` in a format _similar_ to the one returned by `eth_getBlockByNumber/Hash` (please refer to their docs), with some small differences:
  - the block data comes nested inside a `block` attribute.
  - the `transactions` attribute is not returned. We remove the transaction list entirely to avoid unnecessary network traffic.
  - the transaction count is returned in a `transactionCount` attribute.
  - the `logsBloom` attribute comes with `null`. It is a byte blob that is rarely used, so we cap it to avoid unnecessary network traffic.
  - an extra `issuance` attribute returns an `object` with the fields:
    - `blockReward` - the miner reward.
    - `uncleReward` - the total reward issued to uncle blocks.
    - `issuance` - the total ETH issued in this block (miner + uncle rewards).
  - an extra `totalFees` attribute containing the sum of fees paid by senders in this block. Note that due to EIP-1559 this is **NOT** the same amount earned by the miner as block fees since it contains the amount paid as base fee.

### `ots_getBlockTransactions`

Gets paginated transaction data for a certain block. Think of an optimized `eth_getBlockBy*` + `eth_getTransactionReceipt`.

The `transactions` field contains the transaction list with their bodies in a similar format of `eth_getBlockBy*` with transaction bodies, with a few differences:

- the `input` field returns only the 4 bytes method selector instead of the entire calldata byte blob.

The `receipts` attribute contains the transactions receipt list, in the same sort order as the block transactions. Returning it here prevents the caller from making N+1 separate calls (`eth_getBlockBy*` and `eth_getTransactionReceipt`).

For receipts, it differs from the `eth_getTransactionReceipt` object format:

- `logs` attribute returns `null`.
- `logsBloom` attribute returns `null`.

### `ots_searchTransactionsBefore` and `ots_searchTransactionsAfter`

These are address history navigation methods. They are similar, but `ots_searchTransactionsBefore` searches the history backward and `ots_searchTransactionsAfter` searches forward a certain point in time.

They are paginated, so you **MUST** include a page size. Some addresses like exchange addresses or very popular DeFi contracts like a Uniswap router will return millions of results.

They return inbound (`to`), outbound (`from`) and "internal" transactions. By internal it means that if a transaction calls a contract and somewhere in the call stack it sends ETH to the address you are searching for or the address is a contract and it calls a method on it, the transaction is matched and returned in the search results.

Parameters:

1. `address` - The ETH address to be searched.
2. `blockNumber` - It searches for occurrences of `address` before/after `blockNumber`. A value of `0` means you want to search from the most recent block (`ots_searchTransactionsBefore`) or from the genesis block (`ots_searchTransactionsAfter`).
3. `pageSize` - How many transactions it may return. See the detailed explanation about this parameter below.

Returns:

- `object` containing the following attributes:
  - `txs` - An array of objects representing the transaction results. The results are always returned sorted from the most recent to the oldest one (in descending chronological order).
  - `receipts` - An array of objects containing the transaction receipts for the transactions returned in the `txs` attribute.
  - `firstPage` - Boolean indicating this is the first page. It should be `true` when calling `ots_searchTransactionsBefore` with `blockNumber` == 0 (search from `latest`); because the results are in descending order, the search from the most recent block is the "first" one. It should also return `true` when calling `ots_searchTransactionsAfter` with a `blockNumber` which results in no more transactions after the returned ones because it searched forward up to the tip of the chain.
  - `lastPage` - Boolean indicating this is the last page. It should be `true` when calling `ots_searchTransactionsAfter` with `blockNumber` == 0 (search from genesis); because the results are in descending order, the genesis page is the "last" one. It should also return `true` when calling `ots_searchTransactionsBefore` with a `blockNumber` which results in no more transactions before the returned ones because it searched backwards up to the genesis block.

There is a small gotcha regarding `pageSize`. If there are fewer results than `pageSize`, they are just returned as is.

But if there are more than `pageSize` results, they are capped by the last found block. For example, let's say you are searching for Uniswap Router address with a `pageSize` of 25, and it already found 24 matches. It then looks at the next block containing this address's occurrences and there are 5 matches inside the block. They are all returned, so it returns 30 transaction results. The caller code should be aware of this.

Example: get the first 5 transactions that touched Uniswap V1 router (which includes the contract creation).

Request:

```sh
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '
  {
    "jsonrpc":"2.0",
    "id": 1,
    "method":"ots_searchTransactionsAfter",
    "params":
      [
        "0xc0a47dFe034B400B47bDaD5FecDa2621de6c4d95",
        0,
        5
      ]
  }' \
  http://127.0.0.1:8545
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "txs": [
      {
        "blockHash": "0x06a77abe52c486f58696665eaebd707f17fbe97eb54480c6533db725769ce3b7",
        "blockNumber": "0x652284",
        "from": "0xd1c24f50d05946b3fabefbae3cd0a7e9938c63f2",
        "gas": "0xf4240",
        "gasPrice": "0x2cb417800",
        "hash": "0x14455f1af43a52112d4ccf6043cb081fea4ea3a07d90dd57f2a9e1278114be94",
        "input": "0x1648f38e000000000000000000000000e41d2489571d322189246dafa5ebde1f4699f498",
        "nonce": "0x6",
        "to": "0xc0a47dfe034b400b47bdad5fecda2621de6c4d95",
        "transactionIndex": "0x71",
        ...
  }
```

### `ots_getTransactionBySenderAndNonce`

Given a sender address and a nonce, returns the tx hash or `null` if not found. It returns only the tx hash on success, you can use the standard `eth_getTransactionByHash` after that to get the full transaction data.

Parameters:

1. `sender` - The sender ETH address.
2. `nonce` - The sender nonce.

Returns:

- `string` containing the corresponding transaction hash or `null` if it doesn't exist.

Example: get the 4th transaction sent by Vitalik's public address (nonce == 3).

Request:

```sh
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '
  {
    "jsonrpc":"2.0",
    "id": 1,
    "method":"ots_getTransactionBySenderAndNonce",
    "params":
      [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        3
      ]
  }' \
  http://127.0.0.1:8545
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x021304206b2517c3f8f2df07014a55b79aac2ae097488fa807cc88eccd851a50"
}
```

### `ots_getContractCreator`

Given an ETH contract address, returns the tx hash and the direct address who created the contract.

If the address is an EOA or a destroyed contract, it returns `null`.

Parameters:

1. `address` - The ETH address that may contain a contract.

Returns:

- `object` containing the following attributes, or `null` if the address does not contain a contract.
  - `hash` - The tx hash of the transaction who created the contract.
  - `creator` - The address which created the contract. Note that for simple transactions that directly deploy a contract this corresponds to the EOA in the `from` field of the transaction. For deployer contracts, i.e., the contract is created as a result of a method call, this corresponds to the address of the contract which created it.

Example: get the address which deployed the Uniswap V3 Router contract.

Request:

```sh
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '
  {
    "jsonrpc":"2.0",
    "id": 1,
    "method":"ots_getContractCreator",
    "params":
      [
        "0xE592427A0AEce92De3Edee1F18E0157C05861564"
      ]
  }' \
  http://127.0.0.1:8545
```

Response:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "hash": "0xe881c43cd88063e84a1d0283f41ee5348239b259c0d17a7e2e4552da3f4b2bc7",
    "creator": "0x6c9fc64a53c1b71fb3f9af64d1ae3a4931a5f4e9"
  }
}
```
