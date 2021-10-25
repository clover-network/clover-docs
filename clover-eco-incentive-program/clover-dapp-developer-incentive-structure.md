# Clover Developer Incentive Program

The Developer Incentive Program (DIP) is made of two respective implementations, a foundational-layer coinbase rule activation and a following smart contract implementation.

## Coinbase Rule Activation

Clover users contribute to the program indirectly with transaction fees, this is so that a new fee schedule is not committed to the transaction structure itself. Wallet softwares functions the same as usual without breaking backwards compatibility.

The coinbase transaction which spends the block reward and all transaction fees to an address of the validators choosing follows a subsequent transaction where 49 percent of txFee reward is respectively transferred to DIP contract. The amount of total CLV a successful validator can claim for himself is therefore changed from blockReward + txnFees to blockReward + txnFees_51/100. _Whenever a block is propagated, every node will check whether the block adheres to the rules where the sum of all transaction outputs in a block must be equal or smaller than all transaction inputs and the block reward: sum(blockOutputs) ¡ sum(blockInputs) + (blockReward + txnFees51/100) + txnFees\*49/100

## Smart Contract Implementation

External contract registration and reward distribution are done through DIP contract, a trust- less autonomous contract that lives on the Clover parachain. This implementation is made of three main phases; registration, invocation and reward distribution.

### Registration

Third party Clover developers can benefit from the Developer Incentive Program upon registering their compiled contract with DIP contract pre-deployment. An external contract willing to register Developer Incentive Program should include an internal method called transactAndInvokeDIP which is used to trigger reward incrementation, and another internal method called claimRewards which is used to trigger reward settlement.

Right before contract deployment, the respective developer submits contract ABI and contract hexadecimal representation to DIP contract via registerExternalContract method. Given parameters registerExternalContract registers submission upon checking whether the contract is well-formatted and transactAndInvokeDIP is well-structured.

![New Contract Registration Logic](<../.gitbook/assets/image (10).png>)

### Invocation

transactAndInvokeDIP adds a new standard to contracts on the Clover network which can be called internally within the external contract with the additional data provided. Whenever a clover user interacts with a registered external contract, transactAndInvokeDIP invokes DIP contract’s function listenInvokeDIP and triggers an event incrementRewardNonce(address), following the convention set in ERC677.

![Invocation Logic](<../.gitbook/assets/image (11).png>)

### Reward Distribution

Registered third party Clover developers can claim their rewards, in a predefined period of time, upon calling internal claimRewards function which triggers a set of events to distribute a portion of associated rewards based on external contract’s reward nonce and DIP contract’s total reward pool from coinbase txFee rewards.

![Reward Distribution Logic](<../.gitbook/assets/image (12).png>)

