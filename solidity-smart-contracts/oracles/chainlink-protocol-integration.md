# Chainlink Protocol Integration

## Introduction to Price Feeds <a id="introduction-to-price-feeds"></a>

Chainlink Price Feeds are the quickest way to connect your smart contracts to the real-world market prices of assets. They enable smart contracts to retrieve the latest price of an asset in a single call.

Often, smart contracts need to act upon prices of assets in real-time. This is especially true in [DeFi](https://defi.chain.link/). For example, [Synthetix](https://www.synthetix.io/) use Price Feeds to determine prices on their derivatives platform. Lending and Borrowing platforms like [AAVE](https://aave.com/) use Price Feeds to ensure the total value of the collateral.

## Get the Latest Price <a id="get-the-latest-price"></a>

This section explains how to get the latest price of CLV inside smart contracts using Chainlink Price Feeds, on the Binance Smart Chain.

**Solidity Contract**

To consume price data, your smart contract should reference [`AggregatorV3Interface`](https://github.com/smartcontractkit/chainlink/blob/master/evm-contracts/src/v0.6/interfaces/AggregatorV3Interface.sol), which defines the external functions implemented by Price Feeds.

```text
pragma solidity ^0.6.7;

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Clover Chain
     * Aggregator: CLV/USD
     * Address: 0x063eBCD1dB02320814Acc0721e65f14b8755Ff41
     */
    constructor() public {
        priceFeed = AggregatorV3Interface(0x063eBCD1dB02320814Acc0721e65f14b8755Ff41);
    }

    /**
     * Returns the latest price
     */
    function getLatestPrice() public view returns (int) {
        (
            uint80 roundID, 
            int price,
            uint startedAt,
            uint timeStamp,
            uint80 answeredInRound
        ) = priceFeed.latestRoundData();
        return price;
    }
}
```

**Javascript Web3**

```text
const Web3 = require("web3");
const web3 = new Web3("https://rpc.clover.finance");
const aggregatorV3InterfaceABI = [{"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"description","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint80","name":"_roundId","type":"uint80"}],"name":"getRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"latestRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"version","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}];
const addr = "0x063eBCD1dB02320814Acc0721e65f14b8755Ff41";
const priceFeed = new web3.eth.Contract(aggregatorV3InterfaceABI, addr);
priceFeed.methods.latestRoundData().call()
    .then((roundData) => {
        // Do something with roundData
        console.log("Latest Round Data", roundData)
    });
```

**Python Web3**

```text
from web3 import Web3
web3 = Web3(Web3.HTTPProvider('https://rpc.clover.finance'))
abi = '[{"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"description","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint80","name":"_roundId","type":"uint80"}],"name":"getRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"latestRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"version","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}]'
addr = '0x063eBCD1dB02320814Acc0721e65f14b8755Ff41'
contract = web3.eth.contract(address=addr, abi=abi)
latestData = contract.functions.latestRoundData().call()
print(latestData)
```

## Get Historical Price Data <a id="get-historical-price-data"></a>

The most common use case for Price Feeds is to get the latest price. However, [`AggregatorV3Interface`](https://github.com/smartcontractkit/chainlink/blob/master/evm-contracts/src/v0.6/interfaces/AggregatorV3Interface.sol) also exposes functions which can be used to retrieve price data of a previous round ID.

**Solidity Contract**

```text
pragma solidity ^0.6.7;

import "https://github.com/smartcontractkit/chainlink/blob/master/evm-contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract HistoricalPriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Clover Chain
     * Aggregator: CLV/USD
     * Address: 0x063eBCD1dB02320814Acc0721e65f14b8755Ff41
     */
    constructor() public {
        priceFeed = AggregatorV3Interface(0x063eBCD1dB02320814Acc0721e65f14b8755Ff41);
    }

    /**
     * Returns historical price for a round id.
     * roundId is NOT incremental. Not all roundIds are valid.
     * You must know a valid roundId before consuming historical data. 
     * @dev A timestamp with zero value means the round is not complete and should not be used.
     */
    function getHistoricalPrice(uint80 roundId) public view returns (int256) {
        (
            uint80 id, 
            int price,
            uint startedAt,
            uint timeStamp,
            uint80 answeredInRound
        ) = priceFeed.getRoundData(roundId);
        require(timeStamp > 0, "Round not complete");
        return price;
    }
}
```

 **Javascript Web3**

```text
const Web3 = require("web3");

const web3 = new Web3("https://rpc.clover.finance");
const aggregatorV3InterfaceABI = [{"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"description","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint80","name":"_roundId","type":"uint80"}],"name":"getRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"latestRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"version","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}];
const addr = "0x063eBCD1dB02320814Acc0721e65f14b8755Ff41";
const priceFeed = new web3.eth.Contract(aggregatorV3InterfaceABI, addr);

// Valid roundId must be known. They are NOT incremental.
let validId = BigInt("18446744073709554130");

priceFeed.methods.getRoundData(validId).call()
    .then((historicalRoundData) => {
        // Do something with price
        console.log("Historical round data", historicalRoundData);
    })
```

**Python Web3**

```text
from web3 import Web3

web3 = Web3(Web3.HTTPProvider('https://rpc.clover.finance'))
abi = '[{"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"description","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint80","name":"_roundId","type":"uint80"}],"name":"getRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"latestRoundData","outputs":[{"internalType":"uint80","name":"roundId","type":"uint80"},{"internalType":"int256","name":"answer","type":"int256"},{"internalType":"uint256","name":"startedAt","type":"uint256"},{"internalType":"uint256","name":"updatedAt","type":"uint256"},{"internalType":"uint80","name":"answeredInRound","type":"uint80"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"version","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}]'
addr = '0x063eBCD1dB02320814Acc0721e65f14b8755Ff41'
contract = web3.eth.contract(address=addr, abi=abi)

#  Valid roundId must be known. They are NOT incremental.
validRoundId = 18446744073709554130

historicalData = contract.functions.getRoundData(validRoundId).call()
print(historicalData)
```

## API Reference <a id="api-reference"></a>

API reference for [`AggregatorV3Interface`](https://github.com/smartcontractkit/chainlink/blob/develop/evm-contracts/src/v0.6/interfaces/AggregatorV3Interface.sol).

### Functions <a id="functions"></a>

| Name | Description |
| :--- | :--- |
| [decimals]() | The number of decimals in the response. |
| [description]() | The description of the aggregator that the proxy points to. |
| [getRoundData]() | Get data from a specific round. |
| [latestRoundData]() | Get data from the latest round. |
| [version]() | The version representing the type of aggregator the proxy points to. |

#### decimals <a id="decimals"></a>

Get the number of decimals present in the response value.

\`\`\`javascript Solidity function decimals\(\) external view returns \(uint8\)

```text
* `RETURN`: The number of decimals.

#### description

Get the description of the underlying aggregator that the proxy points to.

```javascript Solidity
function description() external view returns (string memory)
```

* `RETURN`: The description of the underlying aggregator.

#### getRoundData <a id="getrounddata"></a>

Get data about a specific round, using the `roundId`.

\`\`\`javascript Solidity function getRoundData\(uint80 \_roundId\) external view returns \( uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound \)

```text
**Parameters**

* `roundId`: The round ID

**Return Values**

* `roundId`: The round ID.
* `answer`: The price.
* `startedAt`: Timestamp of when the round started.
* `updatedAt`: Timestamp of when the round was updated.
* `answeredInRound`: The round ID of the round in which the answer
   * was computed.

#### latestRoundData

Get the price from the latest round.

```javascript Solidity
function latestRoundData() external view 
    returns (
        uint80 roundId, 
        int256 answer, 
        uint256 startedAt, 
        uint256 updatedAt, 
        uint80 answeredInRound
    )
```

**Return Values**

* `roundId`: The round ID.
* `answer`: The price.
* `startedAt`: Timestamp of when the round started.
* `updatedAt`: Timestamp of when the round was updated.
* `answeredInRound`: The round ID of the round in which the answer
* was computed.

#### version <a id="version"></a>

The version representing the type of aggregator the proxy points to.

`javascript Solidity function version() external view returns (uint256)`

* `RETURN`: The version number.

