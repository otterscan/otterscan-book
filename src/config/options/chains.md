# Recognizing non standard chains

By default, Otterscan recognizes several chains, including the Ethereum mainnet and several Ethereum test networks. For other chains, specify either **(1)** a `chainInfo` key in the Otterscan config, or **(2)** create a JSON file accessible at `{assetsURLPrefix}/chains/eip155-{chainId}.json`. In both cases, use the [ethereum-lists](https://github.com/ethereum-lists/chains) structure to describe the properties of the chain:

* `name`: The full name of the network, such as "Ethereum Mainnet".
* `faucets`: A list of faucet URLs which are accessible at the `/faucets` endpoint and navigable from address pages. The special string `${ADDRESS}` can be included in the URL and will be replaced with the address the user navigated from.
* `nativeCurrency`: Describes the native currency of the chain; this is analogous to ETH on the Ethereum mainnet.
  * `name`: Full name of the native currency, e.g. "Ether".
  * `symbol`: Few-character symbol used in trading, e.g. "ETH".
  * `decimals`: Number of decimals; usually 18.

Example:
```json
{
  "name": "Sepolia Testnet",
  "faucets": [],
  "nativeCurrency": {
    "name": "Sepolia Ether",
    "symbol": "sepETH",
    "decimals": 18
  }
}
```
