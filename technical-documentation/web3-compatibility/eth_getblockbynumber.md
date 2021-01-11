---
description: Returns block with given number
---

# eth\_getBlock

```text
web3.eth.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])
```

#### Parameters

1. The block number or block hash. Or the string `"earliest"`, `"latest"` or `"pending"` as in the [default block parameter](https://web3js.readthedocs.io/en/v1.3.0/web3-eth.html#eth-defaultblock).
2. \(optional, default `false`\) If specified `true`, the returned block will contain all transactions as objects. If `false` it will only contains the transaction hashes.
3. \(optional\) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

The block object:

> * `number`: The block number. `null` if a pending block.
> * `hash`: Hash of the block. `null` if a pending block.
> * `parentHash`: Hash of the parent block
> * `sha3Uncles` : SHA3 of the uncles data in the block.
> * `logsBloom`: The bloom filter for the logs of the block. `null` if a pending block.
> * `transactionsRoot`: The root of the transaction trie of the block.
> * `stateRoot` : The root of the final state trie of the block.
> * `miner` : The address of the beneficiary to whom the mining rewards were given.
> * `difficulty` : Integer of the difficulty for this block.
> * `totalDifficulty`: Integer of the total difficulty of the chain until this block.
> * `extraData`: The “extra data” field of this block.
> * `size`: Integer the size of this block in bytes.
> * `gasLimit`: The maximum gas allowed in this block.
> * `gasUsed`: The total used gas by all transactions in this block.
> * `timestamp`: The unix timestamp for when the block was collated.
> * `transactions`: Array of transaction objects, or 32 Bytes transaction hashes depending on the `returnTransactionObjects` parameter.
> * `uncles`: Array of uncle hashes.
> *   author ：
> *   receiptsRoot ：
> *   sealFields ：
>
> #### Example

> ```text
> //block number
> > web3.eth.getBlock(43458).then(console.log);
> > {
>   author: '0xe4a61e41ac64a0e18c22bc3bf0f0e6c9ded3c08d',
>   difficulty: '0',
>   extraData: '0x',
>   gasLimit: 0,
>   gasUsed: 0,
>   hash: '0x0098d4eb6ec05bf6a7c727310b64ae9bfe1c5fb3ff57a3449bcededa00858015',
>   logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
>   miner: '0xE4A61E41ac64a0e18c22bc3bF0F0E6c9DeD3C08d',
>   number: 43458,
>   parentHash: '0x0dd1bb45074703797fbfcb970df49b17e27f72db05af9784279ded0692bbaafd',
>   receiptsRoot: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
>   sealFields: [
>     '0x0000000000000000000000000000000000000000000000000000000000000000',
>     '0x0000000000000000'
>   ],
>   sha3Uncles: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
>   size: 509,
>   stateRoot: '0x341e4f8c138596ef775ee35c82d196b1b97dc5ee2b4c474bff0d0437da9e355c',
>   timestamp: 1609917924,
>   totalDifficulty: null,
>   transactions: [],
>   transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
>   uncles: []
> }
> ```

```text
//block hash
> web3.eth.getBlock("0x0098d4eb6ec05bf6a7c727310b64ae9bfe1c5fb3ff57a3449bcededa00858015",true).then(console.log)
> {
  author: '0xe4a61e41ac64a0e18c22bc3bf0f0e6c9ded3c08d',
  difficulty: '0',
  extraData: '0x',
  gasLimit: 0,
  gasUsed: 0,
  hash: '0x0098d4eb6ec05bf6a7c727310b64ae9bfe1c5fb3ff57a3449bcededa00858015',
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  miner: '0xE4A61E41ac64a0e18c22bc3bF0F0E6c9DeD3C08d',
  number: 43458,
  parentHash: '0x0dd1bb45074703797fbfcb970df49b17e27f72db05af9784279ded0692bbaafd',
  receiptsRoot: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
  sealFields: [
    '0x0000000000000000000000000000000000000000000000000000000000000000',
    '0x0000000000000000'
  ],
  sha3Uncles: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
  size: 509,
  stateRoot: '0x341e4f8c138596ef775ee35c82d196b1b97dc5ee2b4c474bff0d0437da9e355c',
  timestamp: 1609917924,
  totalDifficulty: null,
  transactions: [],
  transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
  uncles: []
}

```

