# PARSIQ-AXS

In this project we utilize PARSIQ Smart Triggers and Google Spreadsheets integration to track and visualize AXS token transfers on Ethereum and Binance Smart Chain that involves most liquid exchanges.

The idea for the project comes from the fact that on the Binance Exchange the highest earning product in Locked staking is AXS with an estimated APY of 131.25% far outperforming any siimilar prooduct on the exchange.

![image](https://user-images.githubusercontent.com/66903336/140820286-7b9a6fd7-2635-4095-9783-1931f02ea1ac.png)

AXS is a game
on multiple chains including Ethereum and Binance Smart Chain

As of Ronin Bridge

In order to justify such returns the company would usually rely on large inflows and minor outflows from the cryptocurrency. Therefore, monitoring the most liquid day-to-day endpoints provides an indicator of the success of the staking solution.

# Triggers

```javascript
stream _
from TokenTransfers

where (@from == AXScontract || @to == AXScontract || @from == Binance14 || @to == Binance14) && (@erc20.symbol == "AXS" || @erc20.symbol == "SLP")

process
    let symb = @erc20.symbol
    let cryptorankFiatRate = getRate(symb)
    let fiat_value = @value * cryptorankFiatRate.value
    let fiat_decimals = @erc20.decimals + cryptorankFiatRate.decimals
    let eth_usd_pair = getChainlinkPriceFeedPair("ETH/USD")
    let score_from = getScore(@from)
    let score_to = getScore(@to)
    emit {@action_type, @block_hash, @code_address, @from, @gas_used, @origin, @to, @value, fiat_value, fiat_decimals, eth_usd_pair, @tx_hash, symb, @block_timestamp, @gas_price, score_from, score_to }
end
```

Several graphs or diagrams, beauty, potential usefulness required.
