# Create Wallet

## Key Management <a id="key-management"></a>

This article is a guide about key management strategy on client side of your Decentralised Application on Clover Chain

### Setup Web3 <a id="setup-web3"></a>

`web3.js` is a javascript library that allows our client-side application to talk to the blockchain. We configure web3 to communicate via Metamask.

`web3.js` doc is [here](https://web3js.readthedocs.io/en/v1.2.2/getting-started.html#adding-web3-js)

### Connect to CLV network <a id="connect-to-bsc-network"></a>

```text
// testnet
const web3 = new Web3('https://rpc.clover.finance');
```

### Set up account <a id="set-up-account"></a>

If the installation and instantiation of web3 was successful, the following should successfully return a random account:

```text
const account = web3.eth.accounts.create();
```

### Recover account <a id="recover-account"></a>

If you have backup the private key of your account, you can use it to restore your account.

```text
const account = web3.eth.accounts.privateKeyToAccount("$private-key")
```

### Full Example <a id="full-example"></a>

```text
const Web3 = require('web3');
async function main() {

    const web3 = new Web3('https://rpc.clover.finance');
    const loader = setupLoader({ provider: web3 }).web3;

    const account = web3.eth.accounts.create();
    console.log(account);
}
```

