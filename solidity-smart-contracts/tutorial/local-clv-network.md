# Local CLV Network

See also : [https://github.com/ethereum/go-ethereum/wiki/Private-network](https://github.com/ethereum/go-ethereum/wiki/Private-network)

## Setting up your BSC Node\(s\) <a id="setting-up-your-bsc-nodes"></a>

### 1. Prereq : Install Geth <a id="1-prereq-install-geth"></a>

Review the guide [here](https://docs.binance.org/smart-chain/developer/fullnode.html)

### 2. Prereq : create /projects <a id="2-prereq-create-projects"></a>

Create a `/projects` symbolic link _\(Note: This step is simply so "/projects" can be used in all other commands, instead you could use full paths, or set an env var\)_

```text
$ mkdir 
$ sudo ln -s  /projects
```

### 3. Create local\_ethereum\_blockchain folder <a id="3-create-local_ethereum_blockchain-folder"></a>

```text
$ mkdir /projects/local_ethereum_blockchain
```

### 4. Create the genesis block config <a id="4-create-the-genesis-block-config"></a>

Create this file : `/projects/local_ethereum_blockchain/genesis.json`

With the following contents :

```text
{
     "config": {
       "chainId": 1000,
       "homesteadBlock": 0,
       "eip155Block": 0,
       "eip158Block": 0
                },
     "nonce": "0x0000000000000061",
     "timestamp": "0x0",
     "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
     "gasLimit": "0x8000000",
     "difficulty": "0x100",
     "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
     "coinbase": "0x3333333333333333333333333333333333333333",
     "alloc": {}
}
```

 \([info about the genesis file](https://ethereum.stackexchange.com/a/2377/2040)\)

### 5. Initialise an Ethereum node <a id="5-initialise-an-ethereum-node"></a>

```text
$ geth --datadir /projects/local_ethereum_blockchain/node1 init /projects/local_ethereum_blockchain/genesis.json
```

### 6. Start that Ethereum node <a id="6-start-that-ethereum-node"></a>

```text
$ geth --datadir /projects/local_ethereum_blockchain/node1 --networkid 1000 console
```

### 7. Initialise another Ethereum node <a id="7-initialise-another-ethereum-node"></a>

```text
$ geth --datadir /projects/local_ethereum_blockchain/node-2 init /projects/local_ethereum_blockchain/genesis.json
```

### 8. Start the 2nd Ethereum node <a id="8-start-the-2nd-ethereum-node"></a>

```text
$ geth --datadir /projects/local_ethereum_blockchain/node-2 --port 30304 --nodiscover --networkid 1000 console
```

### 9. Connect one node to the other <a id="9-connect-one-node-to-the-other"></a>

In one geth console :

In the other console :

## Useful geth commands <a id="useful-geth-commands"></a>

### Node info <a id="node-info"></a>

### Peers <a id="peers"></a>

Show peers

How many peers ?

### Create an account <a id="create-an-account"></a>

You need an account to do be able to do things like mining

_And make sure your remember/save the password!_

### Unlock account <a id="unlock-account"></a>

Neccessary before some actions

```text
> personal.unlockAccount( eth.accounts[0] )
```

### Start mining <a id="start-mining"></a>

The first block may take a while to mine, allow a few minutes

### Stop mining <a id="stop-mining"></a>

### Current block number <a id="current-block-number"></a>

### Details of current block <a id="details-of-current-block"></a>

```text
> eth.getBlock( eth.blockNumber )
```

### Which node minded the last block <a id="which-node-minded-the-last-block"></a>

```text
> eth.getBlock(eth.blockNumber).miner
```

### Account balance, in ether <a id="account-balance-in-ether"></a>

```text
> web3.fromWei(eth.getBalance(eth.accounts[0]))
```

### Transfer ether between accounts <a id="transfer-ether-between-accounts"></a>

First get the account numbers by doing

`> eth.accounts`

Then unlock the account you are sending from

`> personal.unlockAccount( )`

eg.

`> personal.unlockAccount(eth.accounts[0])`

Finally transfer 1 ether

```text
> eth.sendTransaction({from: "", to: "", value: web3.toWei(1, "ether")})
```

### Exit <a id="exit"></a>

\(This will also stop the node from running if it was started using `$ geth console` \(as opposed to `$ geth attach`\)\)

## Connect to other nodes on your network <a id="connect-to-other-nodes-on-your-network"></a>

1. Get the IP of the node : `$ ifconfig|grep netmask|awk '{print $2}'`
2. Get the enode of the node : `> admin.nodeInfo.enode`
3. REPLACE `[::]` in the enode string with the `[]`
4. On your console `> admin.addPeer(< the enode string with the ip address in it>)`

