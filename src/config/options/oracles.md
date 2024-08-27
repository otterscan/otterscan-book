# Price oracles

Otterscan makes use of on-chain [Chainlink data feeds](https://docs.chain.link/data-feeds/price-feeds) for fetching historical price data from Chainlink smart contracts compatible with the [AggregatorV3 interface](https://docs.chain.link/data-feeds/api-reference) which act as price oracles. The native token price is fetched from a Chainlink data feed smart contract.

The Ethereum mainnet has a registry which Otterscan uses to look up token prices for supported tokens. On other chains, and for tokens not in the registry, Otterscan can estimate a token's price using information from on-chain decentralized exchanges.

> **NOTE:** The prices estimated from on-chain decentralized exchanges can be manipulated, especially when tokens have low liquidity or are unable to be traded normally. This price estimation feature returns "best guess" prices; Otterscan certainly cannot guarantee that all tokens with an estimated price have a liquid market or that the prices shown are accurate.

To configure Otterscan's price oracle usage, configure the `priceOracleInfo` key in the config:

- `nativeTokenPrice`: If configured, shows the native token price at the top of the page.
  - `ethUSDOracleAddress`: AggregatorV3-compatible smart contract that returns the price of the native token in USD. If Chainlink does not support your chain, you may consider deploying a smart contract which implements the AggregatorV3 interface yet derives price data from on-chain sources.
  - `ethUSDOracleDecimals`: Number of decimals used by the oracle smart contract.
- `wrappedEthAddress`: The address of the wrapped native token contract, which should match the WETH variable in Uniswap router contracts. This is used to estimate the price of tokens which have at least one Uniswap pool paired with the wrapped native token.
- `uniswapV2`: If configured, uses UniswapV2 pairs as sources of price information.
  - `factoryAddress`: The UniswapV2 factory address.
- `uniswapV3`: If configured, uses UniswapV3 pools as sources of price information. Pools at the 0.01%, 0.05%, 0.3%, and 1% price tiers are queried.
  - `factoryAddress`: The UniswapV3 factory address.

Example for Ethereum mainnet:

```json
{
  "priceOracleInfo": {
    "nativeTokenPrice": {
      "ethUSDOracleAddress": "0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419",
      "ethUSDOracleDecimals": 8
    },
    "wrappedEthAddress": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
    "uniswapV2": {
      "factoryAddress": "0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f"
    },
    "uniswapV3": {
      "factoryAddress": "0x1F98431c8aD98523631AE4a59f267346ea31F984"
    }
  }
}
```
