---
description: Returns an uncles at given block and index
---

# eth\_getUncle

```text
web3.eth.getUncle(blockHashOrBlockNumber, uncleIndex [, returnTransactionObjects] [, callback])
```

#### Parameters

1. The block number or hash. Or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#eth-defaultblock).
2. The index position of the uncle.
3. \(optional, default `false`\) If specified `true`, the returned block will contain all transactions as objects. By default it is `false` so, there is no need to explictly specify false. And, if `false` it will only contains the transaction hashes.
4.  \(optional\) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

the returned uncle

```text

```

