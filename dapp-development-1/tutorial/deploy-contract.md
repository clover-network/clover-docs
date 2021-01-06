# Deploy Contract

## Start Clover dev node

Now we will deploy the Counter smart contract to our clover local dev node. 

First make sure clover is started using below command:

```bash
./clover --dev --alice
```

{% hint style="info" %}
 In the following steps we assume you're using the local clover node. You may need to adjust some parameters if you're connecting to other clover nodes.
{% endhint %}

Once clover is started, you could see it's producing blocks every 6 seconds. 

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

{% hint style="warning" %}
Sometimes the local node stop producing blocks and reports the error "unexpected era change", it normally happens in dev node. To fix this problem, the simplest method is purge the chain using below command and start over.
{% endhint %}

```bash
./clover purge-chain --dev --alice
```

## Setup the private key provider

We need to access our local node using the key keys preimported in the dev chain. We need setup a custom private key provider for truffle. Install required packages using:

```bash
$ yarn add web3-provider-engine ethereumjs-wallet
```

Create `private-provider.js` and set the content as below:

```javascript
const ProviderEngine = require("web3-provider-engine");
const WalletSubprovider = require('web3-provider-engine/subproviders/wallet');
const RpcSubprovider = require('web3-provider-engine/subproviders/rpc');
const EthereumjsWallet = require('ethereumjs-wallet');



function ChainIdSubProvider(chainId) {
  this.chainId = chainId;
}

ChainIdSubProvider.prototype.setEngine = function (engine) {
  const self = this
  if (self.engine) return
  self.engine = engine
}
ChainIdSubProvider.prototype.handleRequest = function (payload, next, end) {
  if (payload.method == "eth_sendTransaction" && payload.params.length > 0 && typeof payload.params[0].chainId == "undefined") {
    payload.params[0].chainId = this.chainId;
  }
  next()
}


function NonceSubProvider() {
}

NonceSubProvider.prototype.setEngine = function (engine) {
  const self = this
  if (self.engine) return
  self.engine = engine
}
NonceSubProvider.prototype.handleRequest = function (payload, next, end) {
  if (payload.method == "eth_sendTransaction") {
    this.engine.sendAsync({
      jsonrpc: "2.0",
      id: Math.ceil(Math.random() * 4415011859092441),
      method: "eth_getTransactionCount",
      params: [payload.params[0].from, "latest"]
    }, (err, result) => {
      const nonce = typeof result.result == "string" ?
        result.result == "0x" ? 0 : parseInt(result.result.substring(2), 16) : 0;
      payload.params[0].nonce = '0x' + (nonce || 0);
      next();
    })
  } else {
    next()
  }
}

function PrivateKeyProvider(privateKey, providerUrl, chainId) {
  if (!privateKey) {
    throw new Error(`Private Key missing, non-empty string expected, got "${privateKey}"`);
  }

  if (!providerUrl) {
    throw new Error(`Provider URL missing, non-empty string expected, got "${providerUrl}"`);
  }

  this.wallet = EthereumjsWallet.default.fromPrivateKey(new Buffer(privateKey, "hex"));
  this.address = "0x" + this.wallet.getAddress().toString("hex");

  this.engine = new ProviderEngine({ useSkipCache: false });

  this.engine.addProvider(new ChainIdSubProvider(chainId));
  this.engine.addProvider(new NonceSubProvider());
  this.engine.addProvider(new WalletSubprovider(this.wallet, {}));
  this.engine.addProvider(new RpcSubprovider({ rpcUrl: providerUrl }));

  this.engine.start();

}


PrivateKeyProvider.prototype.sendAsync = function (payload, callback) {
  return this.engine.sendAsync.apply(this.engine, arguments);
};

PrivateKeyProvider.prototype.send = function () {
  return this.engine.send.apply(this.engine, arguments);
};

module.exports = PrivateKeyProvider;
```

Edit the `truffle-config.js` and set it to below:

```javascript
const pkProvider = require('./private-provider')

const privateKey = '03183f27e9d78698a05c24eb6732630eb17725fcf2b53ee3a6a635d6ff139680'

module.exports = {
  contracts_build_directory: "./src/contract_build",
  compilers: {
    solc: {
      version: '0.5.2'
    }
  },
  networks: {
    development: {
      provider: () => new pkProvider(privateKey, `http://127.0.0.1:9933`, 1337),
      network_id: 1337,
      gas: 55000000,
      gasPrice: 10_000_000_000,
      confirmations: 2,
      timeoutBlocks: 200,
      skipDryRun: true,
    },
  },
};
```

Here we imported the `private-provider` package and added the network section in the configuration. We defined the `development` network by using the `pkProvider` which uses the private key  and connects to the local clover rpc port. 

The private key is defined in the dev chain and has enough CLV for testing. It's important to use the `private-provider.js` here because we need add some customization to make truffle pass the correct information to the clover chain.

Clover node uses the `1337` network id and it supports a higher gas limit.  We specified the gas price in the configuration. because clover limits the gas price to be at least `1 gwei` .  

## Write the migration

Migrations are used to do stuff like contract migration and initialization scripts. We'll just write a simple migration to deploy the Counter contract. 

First create the `migrations` folder

```javascript
$ mkdir migrations
```

Add the `1_deploy_counter.js` file, set the content to:

```javascript
const CounterContract = artifacts.require("Counter");

module.exports = async function(deployer, network) {
  await deployer.deploy(CounterContract)
}
```

We wrote a standard truffle migration which load the `Counter` contract and deploy it to the chain in the migration function. Checkout [truffle migration document](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations) for more details.

## Deploy the counter contract

It's time to deploy counter using below command!

```javascript
$ truffle deploy --network development
```

You should get some output as below:

```javascript
truffle deploy --network development

Compiling your contracts...
===========================
> Everything is up to date, there is nothing to compile.



Starting migrations...
======================
> Network name:    'development'
> Network id:      1337
> Block gas limit: 0 (0x0)


1_deploy_counter.js
===================

   Deploying 'Counter'
   -------------------
   > transaction hash:    0x18eacdeebbd40fec104a3de505e9c1065c89b21ff68664b722834cea9a6f0ea9
   > Blocks: 0            Seconds: 0
   > contract address:    0xeB1c50679f8fe33542C44a4A6D779Fb47886E27f
   > block number:        5
   > block timestamp:     1609225848
   > account:             0xAEd40f2261ba43b4dFFE484265ce82D8fFE2B4DB
   > balance:             9999999.99813969
   > gas used:            186031 (0x2d6af)
   > gas price:           10 gwei
   > value sent:          0 ETH
   > total cost:          0.00186031 ETH

   Pausing for 2 confirmations...
   ------------------------------
   > confirmation number: 1 (block: 6)
   > confirmation number: 2 (block: 7)
   > Saving artifacts
   -------------------------------------
   > Total cost:          0.00186031 ETH


Summary
=======
> Total deployments:   1
> Final cost:          0.00186031 ETH
```

It says that the counter contract was deployed successfully to the network and the deployed contract address is `0xeB1c50679f8fe33542C44a4A6D779Fb47886E27f` . It also include the transaction details like the gas price, gas used and total cost in the output. 

## Interact with the Counter contract in the console

We can interact with the counter contract instance using the truffle console from cli. Start truffle console using:

```bash
$ truffle console --network development
truffle(development)> 
```

Truffle console shows the `truffle(development)>` prompt and waiting for our input.

type below commands and checkout the output:

```bash
truffle(development> let instance = Counter.deployed()
undefined
truffle(development> instance
....
truffle(development> let counterValue = await instance.current_value()
truffle(development> counterValue.toNumber()
0
truffle(development> let result = await instance.inc()
truffle(development>  result
{
  tx: '0xd5ac7b80244d1251782df0dd8411b843e527d1fa92fd580f4849cbc1d9139811',
  receipt: {
    blockHash: '0x75f72177aedfc2e0d27ff20d0b39c5688f320d165095dead0d0ba5986c64f0bb',
    blockNumber: 148,
    contractAddress: null,
    cumulativeGasUsed: 43793,
    from: '0xaed40f2261ba43b4dffe484265ce82d8ffe2b4db',
    gasUsed: 43793,
    internalTransactions: [ [Object] ],
    logs: [],
    logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
    status: true,
    to: '0xeb1c50679f8fe33542c44a4a6d779fb47886e27f',
    transactionHash: '0xd5ac7b80244d1251782df0dd8411b843e527d1fa92fd580f4849cbc1d9139811',
    transactionIndex: 0,
    rawLogs: []
  },
  logs: []
}
truffle(development> let newValue = await instance.current_value()
truffle(development> newValue.toNumber()
1
```

In above commands we can use read the state `current_value` using `instance.current_value()` And we use the `instance.inc()` method to send a transaction which results the current value was increased by 1.

The result of the `instance.inc()` call was the transaction details which includes the block, block hash, events and fee information.

{% hint style="info" %}
The source code of this chapter could be found at the revision `3e76c43d3` in the [`counter-dapp`](https://github.com/clover-network/example-counter-dapp) source repo.
{% endhint %}

