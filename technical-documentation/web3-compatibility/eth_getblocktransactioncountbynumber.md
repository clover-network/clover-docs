---
description: Returns the number of transactions in a block with given block number
---

# eth\_getBlockTransactionCount

```
web3.eth.getBlockTransactionCount(blockHashOrBlockNumber [, callback])
```

#### Parameters

1. The block number or hash. Or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#eth-defaultblock).
2. &#x20;(optional) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

The number of transactions in the given block.

#### Example

```
> web3.eth.getBlockTransactionCount(43458).then(console.log)
> 0
```
