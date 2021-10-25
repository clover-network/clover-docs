---
description: >-
  Clover EVM is based on Frontier, but we make a lot of changes to support
  Clover features. Please see the following details.
---

# Clover EVM

## New Gas Estimation Feature

Clover EVM is based on Frontier, we find that the gas fee estimation under some cases may fail, such as nested smart contract call.  Clover provides a binary search based solution to solve the problem. Please find the details here:

{% embed url="https://github.com/paritytech/frontier/pull/252" %}

## New EVM Economic Incentives

In order to make developer easily do the development on Clover.  Clover EVM support a new economic incentives. The smart contract owner will receive partial of the transaction fee once their contracts are called. Details are:

* Up to 40% of the transaction fee will be sent to the smart contract owner.
* Up to 60% of the transaction fee will be sent to the miner.

## Other EVM Configurations

Clover change the create\_contract\_limit from 0x6000 to 0xc000,  try to reduce the chances that smart contract deployer has to split their big contract for deployment.
