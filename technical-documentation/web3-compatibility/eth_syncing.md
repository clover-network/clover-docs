---
description: Returns an object with data about the sync status or false
---

# eth\_syncing

```
web3.eth.isSyncing([callback])
```

#### Returns

A sync object when the node is currently syncing or `false`:

> * `startingBlock` - `Number`: The block number where the sync started.
> * `currentBlock` - `Number`: The block number where the node is currently synced to.
> * `highestBlock` - `Number`: The estimated block number to sync to.
> * `knownStates` - `Number`: The number of estimated states to download.
> * `pulledStates` - `Number`: The number of already downloaded states.

#### Example

```
> web3.eth.isSyncing().then(console.log)
> false
```
