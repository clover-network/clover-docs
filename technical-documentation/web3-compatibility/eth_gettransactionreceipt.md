---
description: Returns transaction receipt by transaction hash
---

# eth\_getTransactionReceipt

```
web3.eth.getTransactionReceipt(hash [, callback])
```

#### Parameters

1. The transaction hash.
2. (optional) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

&#x20;A transaction receipt object, or `null` if no receipt was found:

> * `status`: `TRUE` if the transaction was successful, `FALSE` if the EVM reverted the transaction.
> * `blockHash`: Hash of the block where this transaction was in.
> * `blockNumber`: Block number where this transaction was in.
> * `transactionHash`: Hash of the transaction.
> * `transactionIndex`: Integer of the transactions index position in the block.
> * `from`: Address of the sender.
> * `to`: Address of the receiver. `null` when it’s a contract creation transaction.
> * `contractAddress`: The contract address created, if the transaction was a contract creation, otherwise `null`.
> * `cumulativeGasUsed`: The total amount of gas used when this transaction was executed in the block.
> * `gasUsed`: The amount of gas used by this specific transaction alone.
> * `logs`: Array of log objects, which this transaction generated.
> * `internalTransactions`:
> * `logsBloom`:
>
> #### Example

> ```
> web3.eth.getTransactionReceipt('0x6d7d750cea87c11ade7852ce02a0a10617f1877995591057f99f66c4d9a350a3').then(console.log)
> > {
>   blockHash: '0x79275dcda076bd6d07774318ae7cd74b9f7141b4e6242b1501b2ca25edb35684',
>   blockNumber: 54550,
>   contractAddress: null,
>   cumulativeGasUsed: 21000,
>   from: '0x063ebcd1db02320814acc0721e65f14b8755ff41',
>   gasUsed: 21000,
>   internalTransactions: [
>     {
>       developer: null,
>       developerReward: null,
>       from: '0x063ebcd1db02320814acc0721e65f14b8755ff41',
>       gasUsed: '0x0',
>       to: '0x69d8fa34508c43c8533a32ab278addde2820556b'
>     }
>   ],
>   logs: [],
>   logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
>   status: true,
>   to: '0x69d8fa34508c43c8533a32ab278addde2820556b',
>   transactionHash: '0x6d7d750cea87c11ade7852ce02a0a10617f1877995591057f99f66c4d9a350a3',
>   transactionIndex: 0
> }
> ```
>
> ####
