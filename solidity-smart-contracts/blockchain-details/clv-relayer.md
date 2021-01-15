# CLV Relayer

## Prepare Fund <a id="prepare-fund"></a>

1. Make sure that you have enough BNB in your account. You can get from[ faucet](http://faucet.clovernode.com/)

If you haven't created your account yet, please follow these [guides](https://docs.binance.org/smart-chain/wallet/metamask.html) to create one first.

* **100BNB** for relayer register
* More than 50BNB for transaction fee

Tip

Currently the bsc-relayer code is not fully prepared. Some features like `db persistence`, `alert`, `prometheus monitor` are still under development. So please don’t modify the configuration about db\_config, alert\_config, instrumentation\_config, admin\_config

## Steps to Install CLV Relayer <a id="steps-to-install-bsc-relayer"></a>

1.Build from source code

Make sure that you have installed [Go 1.13+](https://golang.org/doc/install) and have added `GOPATH` to `PATH` environment variable

```text
git clone https://github.com/binance-chain/bsc-relayer
# Enter the folder bsc was cloned into
cd bsc-relayer
# Comile and install bsc
make build
```

or you can download the pre-build binaries from [release page](https://github.com/binance-chain/smart-chain-binary/tree/master/bsc)

## Get Example Config File <a id="get-example-config-file"></a>

Get example config from this url: [https://github.com/binance-chain/bsc-relayer/blob/master/config/config.json](https://github.com/binance-chain/bsc-relayer/blob/master/config/config.json)

Edit`config.json` and fill your BSC private key to bsc\_config.private\_key, example private key: `AFD8C5D83F148065176268A9D1EE375A10CEE1E74D15985D4CC63E467EC34DA5`

* Binance Chain Configuration:
  * `mnemonic`: Paste the recovery phrase here. Since bsc-relayer will automaticly submit `double-sign` evidence, if it's committed, the reward will be sent to this address
* Binance Smart Chain Configuration: \*

## Start Relayer <a id="start-relayer"></a>

You can start Locally

```text
./bsc-relayer --config-type local --config-path config.json
```

Output:

```text
(base) huangsuyudeMacBook-Pro:mac huangsuyu$ bsc-relayer --config-type local --config-path config.json
2020-05-27 17:01:16 INFO main Start relayer
2020-05-27 17:01:16 INFO SyncProtocol Sync cross chain protocol from https://github.com/binance-chain/bsc-relayer-config.git
2020-05-27 17:01:18 INFO RegisterRelayerHub This relayer has already been registered
2020-05-27 17:01:18 INFO CleanPreviousPackages channelID: 1, next deliver sequence 55 on BSC, next sequence 55 on BC
2020-05-27 17:01:18 INFO CleanPreviousPackages channelID: 2, next deliver sequence 1273 on BSC, next sequence 1273 on BC
2020-05-27 17:01:18 INFO CleanPreviousPackages channelID: 3, next deliver sequence 6 on BSC, next sequence 6 on BC
2020-05-27 17:01:19 INFO CleanPreviousPackages channelID: 8, next deliver sequence 5 on BSC, next sequence 5 on BC
2020-05-27 17:01:19 INFO RelayerDaemon Start relayer daemon
2020-05-27 17:01:19 INFO Serve start admin server at 0.0.0.0:8080
```

Or, dynamic Sync Cross Chain Protocol Configuration from [https://github.com/binance-chain/bsc-relayer-config](https://github.com/binance-chain/bsc-relayer-config)

* Edit config.json and change "cross\_chain\_config.protocol\_config\_type" to "remote". Then relayer will dynamically sync cross chain protocol configuration from this repository: [https://github.com/binance-chain/bsc-relayer-config](https://github.com/binance-chain/bsc-relayer-config)
* Start relayer service

```text
./bsc-relayer --config-type local --config-path config.json
```

### Verify Status <a id="verify-status"></a>

You could call [RelayerHub Contract](https://bscscan.com/address/0x0000000000000000000000000000000000001006) to verify that your relayer is registered. Go to [read contract](https://bscscan.com/address/0x0000000000000000000000000000000000001006#readContract) and call **isRelayer** function. If it returns **true**, then your relayer is working properly.

## Relayer Rewards <a id="relayer-rewards"></a>

1. You can witness the distribution of relayer rewards in the log of system contract: [https://bscscan.com/address/0x0000000000000000000000000000000000001005\#events](https://bscscan.com/address/0x0000000000000000000000000000000000001005#events). According to the design of [Relayer Incentive](https://docs.binance.org/smart-chain/guides/concepts/incentives.html), the rewards will be distributed every 1000 data packages. The total accumulated rewards can be read from [contract](https://bscscan.com/address/0x0000000000000000000000000000000000001005#readContract)the value of `_collectedRewardForHeaderRelayer` and `_collectedRewardForTransferRelayer`.
2. Query your relayer's status

The total accumulated relayed count can be read from [contract](https://bscscan.com/address/0x0000000000000000000000000000000000001005#readContract)the value of `_transferRelayersSubmitCount`

## Stop Relayer <a id="stop-relayer"></a>

To get your locked **100BNB** back, you need to call [RelayerHub Contract](https://bscscan.com/address/0x0000000000000000000000000000000000001006) to unregister your relayer. The fee is **0.1BNB**

* Go to MyEtherWallet and [interact with contract](https://www.myetherwallet.com/interface/interact-with-contract)
* Fill in the contract addresss: **0x0000000000000000000000000000000000001006** with [abi](https://docs.binance.org/smart-chain/system-smart-contract/relayerhub.abi) interface
* Call **unregister** function and leave value in ETH as 0
* Sign your transaction in **MetaMask**

