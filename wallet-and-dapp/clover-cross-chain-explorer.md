# Clover Cross-Chain Explorer

Clover Cross-Chain Explorer \([https://tx.clover.finance/](https://tx.clover.finance/)\) shows all the information about the cross chain transactions

## Cross-chain Transaction Summary

The summary section will show:

* Total cross-chain transaction happened
* Total cross-chain volume in CLV
* Total number of addresses who participate the cross-chain
* Current cross-chain fees.  Clover &lt;-&gt; Ethereum and Clover &lt;-&gt; BSC

Also users can search all the cross-chain transactions by hash or their CLV \(Native token, ERC20, BEP20\) token address

![](../.gitbook/assets/image%20%2881%29.png)

## Cross-chain Transaction Record

cross-chain transaction list will show all the details, like:

* From address, with the source blockchain info
* To address, with the target blockchain info
* Amount of CLV transferred
* The cross-chain transaction time, fee, duration, and status

![Cross-chain Tx List](../.gitbook/assets/image%20%2867%29.png)

## Cross-chain Transaction Details

You can click the cross-chain transaction record to expand the details:

* Burn info, including the transaction hash, block number, block confirmations, etc. You can also view the burn transaction on Etherscan or Subscan
* Mint info
* Claim info, including the claim transaction hash, claim block, claim time, etc.

![Cross-chain Details](../.gitbook/assets/image%20%2868%29.png)

## Claim Your CLV

In case of special case, for example you uninstall the Clover mobile or extension wallet which contains pending cross-chain transactions.  You can use cross-chain transaction explorer to get your CLV back.

There are two ways to claim your CLV:

### Using Cross-Chain Explorer

1. First, click the "Claim CLV" button on the right upper corner:

![Click the &quot;Claim CLV&quot; button](../.gitbook/assets/image%20%2886%29.png)

2. Fill in the claim form:

Blockchain: you need to select the blockchain where the transaction happened.

Transaction Hash: you need to copy your transaction hash from [Etherscan](https://etherscan.io/) or [BscScan](https://bscscan.com/)

CLV Address: **make sure the CLV address is yours, otherwise you may lose your CLV forever!**

Private Key: the private key or seed phrase of your **Ethereum/BSC** account \(Clover will **never save** your private key, it will only be used in your computer, and deleted from any cache once you close the claim dialog!\).

![Fill in your CLV claim request](../.gitbook/assets/image%20%2887%29.png)

3. Confirm the claim:

![Confirm you request](../.gitbook/assets/image%20%2885%29.png)

4. Check your balance after claim succeed:

![Claim with success](../.gitbook/assets/image%20%2888%29.png)

### Using Clover Extension Wallet

* First, you need to install Clover intension wallet and import the same account from which you sent the previous cross-chain transaction
* Connect you wallet to the cross-chain transaction explorer
* Search your address or the pending transaction hash
* Expand the detail, and click the "Claim" button
* Input your claiming CLV address trigger the claim process

![Claim CLV from Cross-chain Transaction Explorer](../.gitbook/assets/image%20%2870%29.png)

Once you input your CLV address, just click "claim" to invoke Clover extension wallet to sign your claim request.

![Sign your Claim Request ](../.gitbook/assets/image%20%2865%29.png)

Once your claim is successful, you can view your claim transaction detail in Subscan.

![Claim Success](../.gitbook/assets/image%20%2866%29.png)



