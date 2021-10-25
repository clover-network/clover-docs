# Wallet Integration QA

## Asset Integration

if you need your blockchain assets supported by Clover Wallet, please provide the javascript SDK for your blockchain. For example:

| Assets | SDK             | Description |
| ------ | --------------- | ----------- |
| DOT    | @polkadot/api   |             |
| SOL    | @solana/web3.js |             |
| ...    |                 |             |

If your chain is EVM compatible, you only need to provide the following information:

* Chain ID
* Chain RPC
* Native Token Symbol
* Native Token Decimal

## dApp Integration

If your dApp is a Solana dApp, please refer to the following integration demo:

{% embed url="https://github.com/Bonfida/audaces-perps-ui/pull/5/commits/7a31c4004dda4dce9cbcdd11c84c0a90e159550e" %}

If your dApp is a EVM compatible dApp, please refer to the following integration demo:

{% embed url="https://github.com/sushiswap/sushiswap-interface/pull/401/commits/aba48d0650813f349f08e8ca61067b15ff2b0f3f" %}
