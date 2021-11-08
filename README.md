# PARSIQ-AXS

In this project we utilize PARSIQ Smart Triggers and Google Spreadsheets integration to track and visualize AXS token transfers on Ethereum and Binance Smart Chain that involves most liquid exchanges.

AXS is an Ethereum token that powers Axie Infinity, a blockchain-based game where players can battle, collect, and build a digital kingdom for their pets. AXS holders can claim rewards for staking their tokens, playing the game, and participating in key governance votes.

The token is available on multiple chains including Ethereum and Binance Smart Chain.

On April 28 2021 the game has made a significant update introducing Ronin Bridge, Axie Infinity Ethereum sidechain. 

The contract has since become the crucial link in the Axie Infinity ecosystem and largely contributes to the prrice action of AXS. With the update the bridge has become the only available option for Axie marketplace, breeding, and morphing contracts.

The idea for the project comes from the fact that on the Binance Exchange the highest earning product in Locked staking is AXS with an estimated APY of 131.25% far outperforming any similar prooduct on the exchange.

![image](https://user-images.githubusercontent.com/66903336/140820286-7b9a6fd7-2635-4095-9783-1931f02ea1ac.png)

In order to justify such returns the company would usually rely on large inflows and minor outflows from the cryptocurrency. Therefore, monitoring the most liquid day-to-day endpoints provides an indicator of the success of the staking solution.

# Triggers

### Trigger for Token transfer from or to Binance Account of SPL or AXS token on Ethereum
Apart from collecting the data from the transaction we collect data using Chainlink.

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

### Trigger for Token transfer from or to Binance Accounts of AXS token on Bsc 

```javascript
stream _
from BscBEP20Transfers

where @token.contract == ContractBSC && (@from in BSCdata || @to in BSCdata )

process
  emit { @from, @to, @value, @token, @transaction, @block }
end
```

# User Data

*Primitives*

![image](https://user-images.githubusercontent.com/66903336/140822828-30294eb4-e2d0-40ed-a3a7-e46cc74794bf.png)

![image](https://user-images.githubusercontent.com/66903336/140823044-0f80457f-e92c-4f86-86fd-903ae6e76a54.png)

*Table*

![image](https://user-images.githubusercontent.com/66903336/140823112-159f545c-bcb1-4177-9f4d-97e7c1eb9a36.png)

![image](https://user-images.githubusercontent.com/66903336/140822678-717d0201-4b88-4ba4-9dd6-378ff47500cf.png)

# Results

* [Binance Smart Chain Data](https://docs.google.com/spreadsheets/d/15TcqKG7zHvXzIhEKgtXlkYvKwdJCZJuSti52J2Q5mNk/edit?usp=sharing)

* [Ethereum Data](https://docs.google.com/spreadsheets/d/1ylJp5Y7eoVZUFRcNT3NapTdZa8b6Zd2geQpWZkEongc/edit?usp=sharing)

# Further Resources

* []()
