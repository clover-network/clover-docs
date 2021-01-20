---
description: >-
  Returns a transaction based on a block hash and the transaction’s index
  position.
---

# eth\_getTransactionByBlockHashAndIndex

```text
web3.eth.getTransactionByBlockHashAndIndex(BlockHash, indexNumber [, callback])
```

#### Parameters

1. A block hash .
2. The transaction’s index position.
3. Optional callback, returns an error object as first parameter and the result as second.

#### Returns

A transaction object its hash `transactionHash`:

> * `hash` : Hash of the transaction.
> * `nonce`: The number of transactions made by the sender prior to this one.
> * `blockHash`: Hash of the block where this transaction was in. `null` if pending.
> * `blockNumber`: Block number where this transaction was in. `null` if pending.
> * `transactionIndex`: Integer of the transactions index position in the block. `null` if pending.
> * `from`: Address of the sender.
> * `to`: Address of the receiver. `null` if it’s a contract creation transaction.
> * `value`: Value transferred in wei.
> * `gasPrice`: Gas price provided by the sender in wei.
> * `gas`: Gas provided by the sender.
> * `input` : The data sent along with the transaction.

#### Example

```text
web3.eth.getTransactionByBlockHashAndIndex("0xfb8d7ee8fb5f4fbebf41a55caa5e988a480a0ce277813cd1ec4443c54f601ddd",0).then(console.log)
> {
    "blockHash":"0xfb8d7ee8fb5f4fbebf41a55caa5e988a480a0ce277813cd1ec4443c54f601ddd",
    "blockNumber":"0x8",
    "chainId":"0x539",
    "creates":null,
    "from":"0xe6206c7f064c7d77c6d8e3ed8601c9aa435419ce",
    "gas":"0x5208",
    "gasPrice":"0x3b9aca00",
    "hash":"0x7893da51d25cad2c10ab946e5b770a21a99b9c9ccff8f595f8222dd9f2e2013b",
    "input":"0x",
    "nonce":"0x0",
    "publicKey":"0x8c9a51a90433508cec6ce29b993284a0e8d0835b38d3ced62216800db588a6d55fa2c114fab798977763ffe94a03b1a591c48d972d4daa6ba7810c80528644f4",
    "r":"0x8f59b2e2a2a07e82067fea3903402a30e7a78176e50827678075271decd19179",
    "raw":"0xf86e80843b9aca00825208942193517101eb10ef22f2fa67ef452f66c51839d3896c6b935b8bbd40000080820a96a08f59b2e2a2a07e82067fea3903402a30e7a78176e50827678075271decd19179a01d0c193a99e5ecce21a40ff18e20e0bf4e1751e5308b3e09acc42d0a53b4b3d7",
    "s":"0x1d0c193a99e5ecce21a40ff18e20e0bf4e1751e5308b3e09acc42d0a53b4b3d7",
    "standardV":"0x1","to":"0x2193517101eb10ef22f2fa67ef452f66c51839d3",
    "transactionIndex":"0x0",
    "v":"0xa96",
    "value":"0x6c6b935b8bbd400000"
    }
```

