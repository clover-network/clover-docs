# Wallet

Clover Chain Wallet injects a global API into websites visited by its users at `window.CloverChain`.

This API specification borrows heavily from API MetaMask provided, considering the massive adoption. So Web3 site developers can easily connect their dApps with the Clover Chain Wallet. The APIs allow websites to request users' Clover Chain addresses, read data from the blockchain the user is connected to, and prompt the users to sign messages and transactions.

The presence of the provider object `window.CloverChain` indicates a Clover Chain/Clover Chain user.

The API this extension wallet provides includes API specified by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) and API defined by [MetaMask](https://docs.metamask.io/guide/ethereum-provider.html) \(including some massively relied legacy ones\).

