---
description: >-
  In order to make Clover as a secure and high performance multi-chain. There
  are lots of test cases have been made to support tha.
---

# Clover Test Cases

## EVM Compatibility Tests

### Balance Tests

1. The Clover EVM should provide the same interface for balance transfer and query.
2. The balance transfer should run correctly and related balance updated.

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/blob/main/tests/test-balance.ts](https://github.com/clover-network/clover-sdk/blob/main/tests/test-balance.ts)

### Block Tests

1. The Clover EVM can return correct genesis block.
2. Clover EVM can support query block by number or hash.
3. The returned block should contain transactions and correct transaction root.
4. The newly generated block should has valid timestamp and includes previous block as parent

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/blob/main/tests/test-block.ts](https://github.com/clover-network/clover-sdk/blob/main/tests/test-block.ts)

### Bloom Tests

The transaction receipt should contains correct bloom data

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/blob/main/tests/test-bloom.ts](https://github.com/clover-network/clover-sdk/blob/main/tests/test-bloom.ts)

### Smart Contract Creation Tests

1. Smart contract can be correctly  created by Clover EVM.
2. Smart contract creation can return transaction hash.

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/blob/main/tests/test-contract.ts](https://github.com/clover-network/clover-sdk/blob/main/tests/test-contract.ts)

### Smart Contract Call Tests

1. Clover EVM should support the invoke of smart contract call.
2. The smart contract call should return expected result.
3. The smart contract call should fail in case of invalid parameters.

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/blob/main/tests/test-contract-methods.ts](https://github.com/clover-network/clover-sdk/blob/main/tests/test-contract-methods.ts)

### Gas Fee Tests

1. Clover EVM should support estimation of gas fee.
2. The gas limit should decrease on next block if gas unused.
3. The estimated gas should be correct for smart contract creation or method call.

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/blob/main/tests/test-gas.ts](https://github.com/clover-network/clover-sdk/blob/main/tests/test-gas.ts)

### Other Tests

1. Clover EVM should support correct nonce query.
2. Clover EVM should return the revert reason in case of failure.
3. Clover EVM should return a valid transaction receipt for a successful or failed transaction.

Test cases can be viewed here: [https://github.com/clover-network/clover-sdk/tree/main/tests](https://github.com/clover-network/clover-sdk/tree/main/tests)

## End to End Tests

In order to test the full functionality of Clover EVM, we provide a script which can automatically deploy Uniswap on Clover for one-shot.

1. It can deploy Uniswap ERC20 token to Clover
2. It can deploy Uniswap V2 Factory smart contract to Clover
3. It can deploy WETH ERC20 token to Clover
4. It can deploy Uniswap V2 Router smart contract to Clover
5. It can deploy Multicall smart contract to Clover

Please find the cool script here: [https://github.com/clover-network/clover-sdk/blob/main/tests/e2e-tests/test/test-uniswap.js](https://github.com/clover-network/clover-sdk/blob/main/tests/e2e-tests/test/test-uniswap.js)

## Complex Gas Fee Tests

Clover EVM is based on Frontier, we find that the gas fee estimation under some cases may fail, such as nested smart contract call.  Clover provides a binary search based solution to solve the problem. Please find the details here:

{% embed url="https://github.com/paritytech/frontier/pull/252" %}

After the fix, Clover EVM can support all kinds of gas estimation correctly, please find the test case here: [https://github.com/clover-network/clover-sdk/blob/main/tests/inner-contract-tests/test/test-inner-contract.js](https://github.com/clover-network/clover-sdk/blob/main/tests/inner-contract-tests/test/test-inner-contract.js)

## Clover Account Binding Tests

Clover has a powerful dual-chain architecture, which will empower its users to build their decentralized apps and digital assets on one blockchain \(Clover EVM powered\) and take advantage of Polkadot chain on the other. Thus Clover account binding is very import, which make users able to do all the staffs under one unified account. Also the bound accounts can share the same balance, as well as the integration of the cool features on the dual-chain.

The related test cases can be found here: [https://github.com/clover-network/clover-sdk/blob/main/tests/account-bind-tests/test-account-bind.js](https://github.com/clover-network/clover-sdk/blob/main/tests/account-bind-tests/test-account-bind.js)

## Clover TPS Tests

In order to test the performance of Clover chain,  test cases have been carried out both on EVM and Clover Parachain.

The EVM TPS tests can be found at: [https://github.com/clover-network/clover-sdk/blob/main/tests/tps-tests/test-web3-tps.js](https://github.com/clover-network/clover-sdk/blob/main/tests/tps-tests/test-web3-tps.js)

The Clover Parachain tests can be found at: [https://github.com/clover-network/clover-sdk/blob/main/tests/tps-tests/test-polkadot-tps.js](https://github.com/clover-network/clover-sdk/blob/main/tests/tps-tests/test-polkadot-tps.js)

