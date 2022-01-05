---
description: Interacting Remix with Clover
---

# Using Remix

This guide shows how to create and deploy a Solidity-based smart contract to a Clover standalone node using the [Remix IDE](https://remix.ethereum.org).&#x20;

Remix is one of the most popular Solidity IDE used to write, compile and debug Solidity code. With Clover’s Ethereum compatibility features, Remix can be used directly with a Clover node.

This guide assumes that you have a running local Clover node running in `--dev` mode, and that you have a [MetaMask](https://metamask.io) installation configured to use this local node. If you don't know how to do it, you can find instructions for running a local Clover node [here](https://clover-network.gitbook.io/portal/quick-start/local-node/setting-up-a-node) and to connect MetaMask to it [here](https://clover-network.gitbook.io/portal/quick-start/local-node/using-metamask).

### Checking Prerequisites <a href="checking-prerequisites" id="checking-prerequisites"></a>

We assume you have followed the guides above, and have a local Clover node producing blocks. It should look like this:

![](../../.gitbook/assets/1608540371482.jpg)

And you should have installed MetaMask connected to your local Clover dev node. You should have at least one account that has a balance. It should look like this (expanded view):

![](<../../.gitbook/assets/image (9).png>)

### Getting Started with Remix <a href="getting-started-with-remix" id="getting-started-with-remix"></a>

Now that we can start with Remix to exercise some advanced functionalities in Clover.

To launch Remix, you need to navigate to [https://remix.ethereum.org/](https://remix.ethereum.org). In the main screen, select Solidity to configure Remix for Solidity development, then navigate to the File Explorers view:

![](../../.gitbook/assets/1.png)

Now we can create a new file to save the Solidity smart contract. Click the "+" button under "File Explorers" at the top left. Give it a name "MyToken.sol" in the popup box.

![](../../.gitbook/assets/2.jpg)

Now paste the following smart contract code into the editor tab on the right side:

```go
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.2;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v3.3.0/contracts/token/ERC20/ERC20.sol";

contract Token is ERC20 {

    constructor (uint256 initialSupply) public ERC20("MyToken", "ABC") {
        _mint(msg.sender, initialSupply);
    }
}
```

This is a simple ERC-20 contract based on the current Open Zeppelin ERC-20 template. It creates MyToken with symbol ABC and mints the entirety of the initial supply to the creator of the contract.

Now the editor should look like this:

![](../../.gitbook/assets/3.jpg)

Now navigate to the compile sidebar option first and then click the “Compile MyToken.sol” button at the bottom left:

![](../../.gitbook/assets/4.jpg)

You will see Remix download all of the Open Zeppelin dependencies and compile the contract.

### Deploying a Contract to Clover Using Remix <a href="deploying-a-contract-to-moonbeam-using-remix" id="deploying-a-contract-to-moonbeam-using-remix"></a>

To deploy the contract, you need to navigate to the Deployment sidebar option on the left. Change the topmost “Environment” dropdown from “JavaScript VM” to “Injected Web3”. Thus you can use the MetaMask injected provider, which will point it to your Clover standalone node.&#x20;

Note that you should allow Remix to access your MetaMask account after selecting "Injected Web3", by clicking the "Next" and then the "Connect" button.

![](../../.gitbook/assets/5.jpg)

Now you should see the account in metamask showing up on the Remix side, ready for deployment. Let’s specify an initial supply of 5M tokens in the box next to the Deploy button. Since this contract uses _the_ default of 18 decimals, the value is `5000000000000000000000000`. &#x20;

![](../../.gitbook/assets/6.jpg)

Then, click the Deploy button. You will need to confirm the contract deployment transaction in Metamask.

![](../../.gitbook/assets/7.jpg)

After the deployment is complete, you will see the transaction record listed in MetaMask. Also, the contract will appear under Deployed Contracts in Remix.

![](../../.gitbook/assets/8.jpg)

After the contract is deployed, you can interact with it from within Remix.

Now you can try clicking on name, symbol, and totalSupply. There should return “MyToken,” “ABC,” and “5000000000000000000000000” respectively. If you copy the address and paste it into the balanceOf field, you should see the entirety of the balance of the ERC20 as belonging to that user.&#x20;

![](../../.gitbook/assets/9.jpg)

### Interacting with a Clover-based ERC-20 from MetaMask <a href="interacting-with-a-moonbeam-based-erc-20-from-metamask" id="interacting-with-a-moonbeam-based-erc-20-from-metamask"></a>

To add the newly deployed ERC-20 tokens, you need to copy the contract's address from Remix first. Then back in MetaMask, click on “Add Token” as shown below. Make sure you are in the account that deployed the token contract.

![](../../.gitbook/assets/10.jpg)

Paste the copied contract address into the “Custom Token” field. The “Token Symbol” and “Decimals of Precision” fields should automatically show up.

![](../../.gitbook/assets/11.jpg)

Then click the “Next” and then the “Add Tokens” button, you should see a balance of 5M MyTokens in MetaMask:

![](../../.gitbook/assets/12.jpg)

Now we can send some of these ERC-20 tokens to the other account. Hit “send” to initiate the transfer of 500 MyTokens. Select the destination account and hit “next,” you will be asked to confirm this transaction as shown below.

![](../../.gitbook/assets/13.jpg)

Click “Confirm” to send. After the transaction is complete, you will see a confirmation and a reduction of the account balance from the sender account in MetaMask:

![](../../.gitbook/assets/14.jpg)

If you own the account which you send to, you can check the account balance to verify that the transaction has been successfully.
