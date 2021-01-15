# Remix

Deploys a BEP20 smart contract with a message, and renders it in the front-end. You can interact with the smart contract easily!

This dapp implements a "Hello World" style application that echoes a message passed to the contract to the front end. This tutorial is intended to be followed using the online IDE available at [Remix IDE](https://remix.ethereum.org/).

### Setting up [Remix IDE](https://remix.ethereum.org/) <a id="setting-up-remix-ide"></a>

* Remix is an online IDE to develop smart contracts.
* You need to choose Solidity Compiler and Deploy and Run Transactions.

![img](https://lh3.googleusercontent.com/BUilfLHVuRsIwaPDCry2yk8g58oMZfq65Nf-Ihc7g8rYtp7BjlxJWiwBQj0BK8e8ZsWET4zPcwdKYCcOn1mdsnFaH60E2Uhj5JEyhMz1-f-_aGwupbPbwaoMRZvB0NATWjPQadNT)

* Go to File Explorers, And Create a new file, Name it MegaCoin.sol
* Copy/Paste the Smart contract below into the newly created file `MegaCoin.sol`

## The smart contract <a id="the-smart-contract"></a>

* Create new contract BEP20Token.sol and copy contract code from the bep20 token template [here](https://docs.binance.org/smart-chain/developer/BEP20Token.template)
* Modify “name”, “symbol”, “decimals” and “totalSupply” according to your requirements.

![img](https://lh4.googleusercontent.com/hgxDh_hXCFKwwlkAYG6h9qfxvzyeeD3k-t3tNBD-VSvwTtM4AnaFylZ6SjSmfTKCuIqhs66Z9vi7mRplIfN5ER7n1yMz0EKpO_RDOcTQTrsh5R1DC0doVC7FT05Hu2bboM2o57Qg)

The first line, `pragma solidity ^0.5.16` specifies that the source code is for a Solidity version greater than 0.5.16. [Pragmas](https://solidity.readthedocs.io/en/latest/layout-of-source-files.html#pragma) are common instructions for compilers about how to treat the source code \(e.g., pragma once\).

A contract in the sense of Solidity is a collection of code \(its functions\) and data \(its state\) that resides at a specific address on the Ethereum blockchain. Learn more about the [constructor](https://solidity.readthedocs.io/en/latest/contracts.html#constructor) and [memory](https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html#storage-memory-and-the-stack) in the docs.

### Compile Smart Contract <a id="compile-smart-contract"></a>

* Step1: Click button to switch to compile page
* Step2: Select “BEP20Token” contract
* Step3: Enable “Auto compile” and “optimization”
* Step4: Click “ABI” to copy the contract abi and save it.

![img](https://lh6.googleusercontent.com/qY_5g3ZMnJca6n84W2JxIoBvd8iHRQ0qkOQuJ60pRIcKvgZB5-bXcGq6gS7dFwA5rYXbiS2NyaUQ1Qptcagqa0pb7kmq_S-Dh8drA4R-hDg9_NVp1zPl-tmqIDanlcgLibaR3CV7)

Now, We have to deploy our smart contract on Binance Smart Chain Network. For that, we have to connect to web3 world, this can be done my many services like Metamask, Brave, Portis etc. We will be using Metamask. Please follow this [tutorial to setup a Metamask Account](https://docs.binance.org/smart-chain/wallet/metamask.html).

* Open Metamask and select Custom RPC from the networks dropdown
* Go to setting page 

![img](https://lh5.googleusercontent.com/NqWPIv1MrMJ-W2wDKjxtdxcdFhDwiqhsZ6G6MY6FQnhxPTCCPfPHBJ59vBl1ddxpbfV11ufETWAolV1s9YjCYHPeJCKW1S-sr8gfjcFt3swXM-p3IgafNBqPZ86DvThK-I9gKbrw)

* Add a new network

![](../../.gitbook/assets/wechate6b1d6dd3249493dbdc35ce2a399c93a.png)

* Testnet \* ChainID: 1137 \* Symbol: CLV \* Block Explorer: [https://rpc.clover.finance](https://rpc.clover.finance)
* Go ahead and click save
* Copy your address from Metamask
* Head over to [Faucet](http://faucet.clovernode.com/) and request test CLV
* Now, let's Deploy the Smart Contract on Clover Chain
* Select Injected Web3 in the Environment dropdown and your contract

![](../../.gitbook/assets/wechat3992b3176f68921425976a75691e18d9.png)

* Accept the Connection Request!

![](../../.gitbook/assets/wechatb79a46a51fa05f007a8e5d43650fd6b4.png)

* Once Metamask is connected to Remix, the ‘Deploy’ transaction would generate another metamask popup that requires transaction confirmation.

![](../../.gitbook/assets/wechatfa83faa624300261955ccc46843ef9c6.png)

**Congratulations!** You have successfully deployed a BEP20 Contract. Now you can interact with the Smart Contract. 

