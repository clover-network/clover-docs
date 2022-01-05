---
description: Interacting with a Clover Node Using MetaMask
---

# Using MetaMask

This guide will show you how to use MetaMask wallet connecting to a self-run Clover standalone node, and sending tokens between accounts. There are two ways to interact with Clover: using Substrate RPC endpoints, or by using Web3-compatible RPC endpoints, which is served from the same Substrate RPC RPC server. In this tutorial, we will use Web3 RPC endpoints.

### Installing MetaMask Chrome Extension <a href="#install-the-metamask-extension" id="install-the-metamask-extension"></a>

First of all, install the [MetaMask](https://metamask.io) Extension from the Chrome Store. After installation is done, follow the "Get Started" guide to create a wallet, set a password, and keep your secret backup phrases in a secure place.&#x20;

Now import the development account:

![](../../.gitbook/assets/image.png)

The information for the pre-funded development account for standalone build are:

* Private key: `0xa504b64992e478a6846670237a68ad89b6e42e90de0490273e28e74f084c03c8`
* Public address: `0xe6206C7f064c7d77C6d8e3eD8601c9AA435419cE`

Select “Private Key” and paste the key on the "Import" page, then click the "import" button.

![](<../../.gitbook/assets/image (2).png>)

You will see an imported “Account 2” looks like this:

![](<../../.gitbook/assets/image (3).png>)

### Connecting to the Local Clover Node <a href="#connect-to-the-local-moonbeam-node" id="connect-to-the-local-moonbeam-node"></a>

Now connect MetaMask to your locally running Clover node. It should be producing blocks now:

![](../../.gitbook/assets/1608540371482.jpg)

Back to MetaMask, navigate to Settings -> Networks -> Add Network. Fill in the following Contents:

* Network Name: `Clover Dev`
* New RPC URL: `http://127.0.0.1:9933`
* ChainID: `1023`
* Symbol (Optional): `CLV`

![](<../../.gitbook/assets/image (4).png>)

When done, click on the "save" button. MetaMask should be connected to the local Clover standalone node via its Web3 RPC. Now you should see the Clover dev account with a balance of 10,000,000 CLV.

![](<../../.gitbook/assets/image (5).png>)

### Initiating a Transfer <a href="#initiating-a-transfer" id="initiating-a-transfer"></a>

Now we can try sending tokens with MetaMask. Let's transfer from this dev account to the account we just created while setting up MetaMask, so that we can simply use the “Transfer between my accounts” option.&#x20;

First let’s send 100 tokens. Input an amount of 100 ETH and leave all other settings as default.&#x20;

Note that the token name may be changed to ETH when you re-login,  which is OK for the development purpose.

![](<../../.gitbook/assets/image (6).png>)

Then Click the "Confirm" button to confirm the transaction:

![](<../../.gitbook/assets/image (7).png>)

Once done, you will see the status as “pending” until it is confirmed after a short period.

![](<../../.gitbook/assets/image (8).png>)

The Account 2 balance should now have been decreased by the amount sent plus a gas fee. When switch to Account 1, you should see the 100 tokens has arrived:

![](<../../.gitbook/assets/image (9).png>)

