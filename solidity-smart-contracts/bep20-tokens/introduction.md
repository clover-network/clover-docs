# Introduction

## BEP20 Token <a id="bep20-token"></a>

A BEP20 token must implement the interface `IBEP20` in [IBEP20.sol](https://docs.binance.org/smart-chain/developer/IBEP20.sol). This is a template contract [BEP20Token.template](https://docs.binance.org/smart-chain/developer/BEP20Token.template). Users just need to fill in `_name`, `_symbol`, `_decimals` and `_totalSupply` according to their own requirements:

```text
  constructor() public {
    _name = {{TOKEN_NAME}};
    _symbol = {{TOKEN_SYMBOL}};
    _decimals = {{DECIMALS}};
    _totalSupply = {{TOTAL_SUPPLY}};
    _balances[msg.sender] = _totalSupply;

    emit Transfer(address(0), msg.sender, _totalSupply);
  }
```

Then users can use [Remix IDE](https://remix.ethereum.org/) and [Metamask](https://docs.binance.org/smart-chain/wallet/metamask.html) to compile and deploy the BEP20 contract to CLV.

### Interact with Contract with [Web3](https://www.npmjs.com/package/web3) and NodeJS. <a id="interact-with-contract-with-web3-and-nodejs"></a>

#### Connect to Clover Chain's public RPC endpoint <a id="connect-to-binance-smart-chains-public-rpc-endpoint"></a>

```text
const Web3 = require('web3');

// testnet
const web3 = new Web3('https://rpc.clover.finance');
```

#### Create a wallet <a id="create-a-wallet"></a>

```text
web3.eth.accounts.create([entropy]);
```

#### Output:

```text
web3.eth.accounts.create();
{
  address: '0x926605D0729a968266f1BB299d8Df0471C4F5367',
  privateKey: '0x6b4618539d95f205f33e916e89404b301dde545c0c4acc181fd0c0b42708bad3',
  signTransaction: [Function: signTransaction],
  sign: [Function: sign],
  encrypt: [Function: encrypt]
}
```

#### Recover a wallet <a id="recover-a-wallet"></a>

```text
const account = web3.eth.accounts.privateKeyToAccount("0xe500f5754d761d74c3eb6c2566f4c568b81379bf5ce9c1ecd475d40efe23c577")
```

#### Check balance <a id="check-balance"></a>

```text
web3.eth.getBalance(holder).then(console.log);
```

#### Output:

The balance will be bumped by e18 for CLV.

```text
6249621999900000000
```

#### Create transaction <a id="create-transaction"></a>

**Parameters**

* Object - The transaction object to send:
* from - String\|Number: The address for the sending account. Uses the web3.eth.defaultAccount property, if not specified. Or an address or index of a local wallet in web3.eth.accounts.wallet.
* to - String: \(optional\) The destination address of the message, left undefined for a contract-creation transaction.
* value - Number\|String\|BN\|BigNumber: \(optional\) The value transferred for the transaction in wei, also the endowment if it’s a contract-creation transaction.
* gas - Number: \(optional, default: To-Be-Determined\) The amount of gas to use for the transaction \(unused gas is refunded\).
* gasPrice - Number\|String\|BN\|BigNumber: \(optional\) The price of gas for this transaction in wei, defaults to web3.eth.gasPrice.
* data - String: \(optional\) Either a ABI byte string containing the data of the function call on a contract, or in the case of a contract-creation transaction the initialisation code.
* nonce - Number: \(optional\) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.

```text
    // // Make a transaction using the promise
    web3.eth.sendTransaction({
        from: holder,
        to: '0x0B75fbeB0BC7CC0e9F9880f78a245046eCBDBB0D',
        value: '1000000000000000000',
        gas: 5000000,
        gasPrice: 18e9,
    }, function(err, transactionHash) {
      if (err) {
        console.log(err);
        } else {
        console.log(transactionHash);
       }
    });
```

