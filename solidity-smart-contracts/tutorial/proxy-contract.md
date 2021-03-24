---
description: 'from https://docs.binance.org/smart-chain/developer/upgrade/proxy.html'
---

# Proxy Contract

## What are Upgradeable Contracts? <a id="what-are-upgradeable-contracts"></a>

Smart contracts in EVM are designed to be immutable. Once you create them there is no way to modify them, effectively acting as an unbreakable contract among participants.What do I do if I want to expand the functionality of my contracts? What if there is a bug in the contract that leads to a loss of funds? What if a vulnerability in the Solidity compiler is discovered? Here’s what you’d need to do to fix a bug in a contract you cannot upgrade:

* Deploy a new version of the contract
* Manually migrate all state from the old one contract to the new one \(which can be very expensive in terms of gas fees!\)
* Update all contracts that interacted with the old contract to use the address of the new one
* Reach out to all your users and convince them to start using the new deployment \(and handle both contracts being used simultaneously, as users are slow to migrate\)

There are several approaches that allow us to make some changes to smart contracts.

**Separate logic and data**

By using this approach, data will be read from a designated data contract directly. This is a rather common approach that is also used outside of Solidity. One of the main disadvantages of this approach is that you cannot change the interface of contracts external to the entire system, and you cannot add or remove functions.

**Delegatecall Proxy**

`delegatecall` opcode was implemented in [EIP-7](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7.md). It is possible to delegate execution to other contract, but execution context stays the same. As with delegatecall, the msg.sender will remain that of the caller of the proxy contract. One of the main disadvantages of this approach is that contract code of the proxy will not reflect the state that it stores.

## Writing Upgradeable CIP20 Contracts <a id="writing-upgradeable-bep20-contracts"></a>

It’s worth mentioning that these restrictions have their roots in how the Ethereum VM works, and apply to all projects that work with upgradeable contracts, not just OpenZeppelin Upgrades.

### Initializers <a id="initializers"></a>

You can use your Solidity contracts in the OpenZeppelin Upgrades without any modifications, except for their constructors. Due to a requirement of the proxy-based upgradeability system, no constructors can be used in upgradeable contracts. To learn about the reasons behind this restriction, head to [Proxies](https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies#the-constructor-caveat).

This means that, when using a contract with the OpenZeppelin Upgrades, you need to change its constructor into a regular function, typically named initialize, where you run all the setup logic:

```text
pragma solidity ^0.6.0;
  import "@openzeppelin/contracts/proxy/TransparentUpgradeableProxy.sol";
  contract CIP20UpgradeableProxy is TransparentUpgradeableProxy {
    constructor(address logic, address admin, bytes memory data) TransparentUpgradeableProxy(logic, admin, data) public {
    }}
```

 OpenZeppelin Upgrades provides an Initializable base contract that has an initializer modifier to prevent a contract from being _initialized_ multiple times: https://github.com/clover-network/canonical-upgradeable-cip20/blob/47ed7a710e6e86bdc85f2118bf63fc892e3b7716/contracts/CIP20TokenImplementation.sol\#L37

```text
 /**
     * @dev sets initials supply and the owner
     */
function initialize(string memory name, string memory symbol, uint8 decimals, uint256 amount, bool mintable, address owner) public initializer {
        _owner = owner;
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
        _mintable = mintable;
        _mint(owner, amount);
    }
```

 CIP20 contract initializes the token’s name, symbol, and decimals in its constructor. You should not use these contracts in your CIP20 Upgrades contract. , make sure to use the `upgradableCIP20implementation` that has been modified to use initializers instead of constructors. https://github.com/clover-network/clv-genesis-contract/blob/42922472b43397fbca9d0c84c7f72fbfaf39efc3/contracts/cip20\_template/CIP20Token.template\#L351

```text
constructor() public {
    _name = {{TOKEN_NAME}};
    _symbol = {{TOKEN_SYMBOL}};
    _decimals = {{DECIMALS}};
    _totalSupply = {{TOTAL_SUPPLY}};
    _balances[msg.sender] = _totalSupply;

  }
```

## Using Truffle <a id="using-truffle"></a>

### Setting up the Environment <a id="setting-up-the-environment"></a>

We will begin by creating a new npm project:

```text
mkdir mycontract && cd mycontract
```

 Then,

### Installation <a id="installation"></a>

We will install Truffle.

```text
npm install --save-dev truffle
npm install --save-dev @openzeppelin/contracts
npm install --save-dev zeppelin-solidity
```

When running Truffle select the option to “Create a truffle-config.js”

### Create upgradeable contract <a id="create-upgradeable-contract"></a>

This example token has a fixed supply that is minted to the deployer of the contract.

https://github.com/clover-network/canonical-upgradeable-cip20/blob/master/contracts/CIP20TokenImplementation.sol

```text
const CIP20TokenImplementation = artifacts.require("CIP20TokenImplementation");const CIP20TokenFactory = artifacts.require("CIP20TokenFactory");
const Web3 = require('web3');const web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));
const fs = require('fs');
module.exports = function(deployer, network, accounts) { deployer.then(async () => {  await deployer.deploy(CIP20TokenImplementation);  await deployer.deploy(CIP20TokenFactory, CIP20TokenImplementation.address); });};
```

### Test the contract locally <a id="test-the-contract-locally"></a>

To test upgradeable contracts we should create unit tests for the implementation contract, along with creating higher level tests for testing interaction via the proxy.

```text
contract('Upgradeable CIP20 token', (accounts) => {  it('Create Token', async () => {    const CIP20TokenFactoryInstance = await CIP20TokenFactory.deployed();    cip20FactoryOwner = accounts[0];    cip20Owner = accounts[1];    proxyAdmin = accounts[0];
    const tx = await CIP20TokenFactoryInstance.createP20Token("CLV Token", "CLV", 18, web3.utils.toBN(1e18), true, cip20Owner, proxyAdmin, {from: cip20FactoryOwner});    truffleAssert.eventEmitted(tx, "TokenCreated",(ev) => {      cip20TokenAddress = ev.token;      return true;    });
  });
```

### Transfer Control <a id="transfer-control"></a>

You cann change the proxy owner to another address.

```text
let event = await cip20proxy.methods.changeAdmin(newAdmin).send({from: proxyAdmin});
cip20proxy.getPastEvents("AdminChanged", {fromBlock: 0, toBlock: "latest"}).then(console.log)
```

### Transfer Owner <a id="transfer-owner"></a>

You cann change the CIP20 token owner to another address.

```text
    await cip20.methods.transferOwnership(accounts[5]).send({from: accounts[1]});
    const owner = await cip20.methods.getOwner().call({from: accounts[5]});
```

### Deploy on Testnet <a id="deploy-on-testnet"></a>

Create the following `2_cip20.js` script in the migrations directory.

```text
module.exports = function(deployer, network, accounts) { deployer.then(async () => {  await deployer.deploy(CIP20TokenImplementation);  await deployer.deploy(CIP20TokenFactory, CIP20TokenImplementation.address); });};
```

 You can first deploy our contract to a local test \(such as ganache-cli\) and manually interact with it, then deploy your contract to a public test network.

```text
$ npx truffle console --network ganache
```

 We can interact with our contract using the Truffle console.

```text
truffle(ganache)> CIP20TokenFactoryInstance = await CIP20TokenFactory.deployed();undefinedtruffle(ganache)> await CIP20TokenFactoryInstance.createCIP20Token("CLV Token", "CLV", 18, web3.utils.toBN(1e18), true, {address1}, {address2});
```

Run `truffle migrate` with the CLV testnet to deploy. We can see our implementation contract 'CIP20TokenImplementation' and the 'CIP20TokenFactory' being deployed.

```text
Deploying 'CIP20TokenImplementation'
   ------------------------------------
   > transaction hash:    0xdcd37a388bf9b2f822eff5b816bd4c9db80bc4f6046e3f922cedca12162d46d9
   > Blocks: 3            Seconds: 8
   > contract address:    0xB3fbaf029580145885e915B3CAeEd259Edb9DfE1
   > block number:        5174292
   > block timestamp:     1609990661
   > account:             0x133D144F52705cEb3f5801B63b9EBcCF4102f5Ed
   > balance:             10.648947766
   > gas used:            1147250 (0x118172)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.022945 ETH

   Pausing for 5 confirmations...
   ------------------------------
   > confirmation number: 2 (block: 5174294)
   > confirmation number: 3 (block: 5174295)
   > confirmation number: 5 (block: 5174297)

   Deploying 'CIP20TokenFactory'
   -----------------------------
   > transaction hash:    0x821c8355aaecc36a9f7fe50d2b3722c840047883a6bf500343393554d8ce3696
   > Blocks: 3            Seconds: 8
   > contract address:    0xDC1015512AbBC71e57a607A121a4aC9CF05D89BC
   > block number:        5174300
   > block timestamp:     1609990685
   > account:             0x133D144F52705cEb3f5801B63b9EBcCF4102f5Ed
   > balance:             10.629661146
   > gas used:            964331 (0xeb6eb)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.01928662 ETH
```

### Create a new version of our implementation <a id="create-a-new-version-of-our-implementation"></a>

After a period of time, we decide that we want to add functionality to our contract. In this guide we will add an `whitelist` function.

Create the new implementation, `CIP2_V2.sol` in your contracts directory with the following Solidity code.

```text
/**   * @dev sets multiple whitelist address   */
function multiWhitelistAdd(address[] memory addresses) external onlyOwner {
        for (uint256 i = 0; i < addresses.length; i++) {
            whitelist[addresses[i]] = true;
        }
    }
 /**
     * @dev remove whitelisted address
     */
    function multiWhitelistRemove(address[] memory addresses) external onlyOwner {
        for (uint256 i = 0; i < addresses.length; i++) {
            whitelist[addresses[i]] = false;
        }
    }
 /**
     * @dev check if is a whitelist address
     */
    function isInWhitelist(address a) internal view returns (bool) {
        return whitelist[a];
    }
```

### Test the upgrade locally <a id="test-the-upgrade-locally"></a>

To test our upgrade we should create unit tests for the new implementation contract, along with creating higher level tests for testing interaction via the proxy, checking that state is maintained across upgrades.

We will create unit tests for the new implementation contract. We can add to the unit tests we already created to ensure high coverage.

Create uograde.test.js in your test directory with the following JavaScript.

```text
let tx = await cip20proxy.methods.upgradeTo(newInstance.address).send({from: proxyAdmin});
 cip20proxy.getPastEvents("Upgraded", {fromBlock: 0, toBlock: "latest"}).then(console.log)
```

