---
description: >-
  Returns the hash of the current block, the seedHash, and the boundary
  condition to be met
---

# eth\_getWork

```
web3.eth.getWork([callback])
```

#### Parameters

1. (optional) Optional callback, returns an error object as first parameter and the result as second.

#### Returns

&#x20;the mining work with the following structure:

> * current block header pow-hash
> * the seed hash used for the DAG.
> * the boundary condition (“target”), 2^256 / difficulty.
>
> #### Example

```
web3.eth.getWork().then(console.log)
> [
  '0x0000000000000000000000000000000000000000000000000000000000000000',
  '0x0000000000000000000000000000000000000000000000000000000000000000',
  '0x0000000000000000000000000000000000000000000000000000000000000000'
]
```
