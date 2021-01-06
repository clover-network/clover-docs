---
description: Interacting with a Clover Node Using MetaMask
---

# Using MetaMask

This guide outlines steps for connecting MetaMask to a self-contained Clover standalone node, to send tokens between accounts. You can interact with Clover in two ways: using Substrate RPC endpoints, or by using Web3-compatible RPC endpoints. The latter endpoints are currently being served from the same RPC server as the Substrate RPCs. In this tutorial, we will use the Web3 RPC endpoints to interact with Clover.

### Install the MetaMask Extension <a id="install-the-metamask-extension"></a>

First, we start with a fresh and default [MetaMask](https://metamask.io/) installation from the Chrome store. After downloading, installing and initializing the extension, follow the "Get Started" guide. In there, you need to create a wallet, set a password, and store your secret backup phrase \(this gives direct access to your funds, so make sure to store these in a secure place\). Once completed, we will import the development account:

![](../../.gitbook/assets/image.png)

The details for the development account that comes pre-funded for this standalone build are as follows:

* Private key: `0xa504b64992e478a6846670237a68ad89b6e42e90de0490273e28e74f084c03c8`
* Public address: `0xe6206C7f064c7d77C6d8e3eD8601c9AA435419cE`

On the import screen, select “Private Key” and paste in the key listed above:

![](../../.gitbook/assets/image%20%283%29.png)

You should end up with an imported “Account 2” that looks like this:

![](../../.gitbook/assets/image%20%285%29.png)

### Connect to the Local Clover Node <a id="connect-to-the-local-moonbeam-node"></a>

Now let’s connect MetaMask to our locally running Clover node, which should be producing blocks:

![](../../.gitbook/assets/1608540371482.jpg)

If so, in MetaMask, navigate to Settings -&gt; Networks -&gt; Add Network and fill in the following details:

* Network Name: `Clover Dev`
* New RPC URL: `http://127.0.0.1:9933`
* ChainID: `1337`
* Symbol \(Optional\): `CLV`

![](../../.gitbook/assets/image%20%288%29.png)

When you hit "save" and exit the network settings screen, MetaMask should be connected to the local Clover standalone node via its Web3 RPC, and you should see the Clover dev account with a balance of 10,000,000 CLV.

![](../../.gitbook/assets/image%20%286%29.png)

### Initiating a Transfer <a id="initiating-a-transfer"></a>

Let’s try sending some tokens with MetaMask.

For simplicity, we will transfer from this dev account to the one created while setting up MetaMask. Consequently, we can use the “Transfer between my accounts” option. Let’s transfer 100 tokens and leave all other settings as they are \(The token name may be changed to ETH when you re-login Metamask,  that is OK for development purpose,  later we can switch to Clover wallet to make it displayed correctly\):

![](../../.gitbook/assets/image%20%287%29.png)

Confirm the transaction:

![](../../.gitbook/assets/image%20%281%29.png)

Once you have submitted the transaction, you will see it “pending” until it is confirmed, as shown in the following image:

![](../../.gitbook/assets/image%20%284%29.png)

Note that the Account 2 balance has been decreased by the sent amount + gas fees. Flipping over to Account 1, we see the 100 sent tokens have arrived:

![](../../.gitbook/assets/image%20%289%29.png)



