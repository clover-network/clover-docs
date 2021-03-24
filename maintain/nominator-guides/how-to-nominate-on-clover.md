---
description: >-
  from
  https://wiki.polkadot.network/docs/en/maintain-guides-how-to-nominate-polkadot
---

# How to Nominate on Clover

> The following information applies to the Clover network.

Nominators are one type of participant in the staking subsystem of Clover. They are responsible for appointing their stake to the validators who are the second type of participant. By appointing their stake, they are able to elect the active set of validators and share in the rewards that are paid out.

While the validators are active participants in the network that engage in the block production and finality mechanisms, nominators take a more passive role with a "set-it-and-forget-it" approach. Being a nominator does not require running a node of your own or worrying about online uptime. However, a good nominator performs due diligence on the validators that they elect. When looking for validators to nominate, a nominator should pay attention to their own reward percentage for nominating a specific validator - as well as the risk that they bare of being slashed if the validator gets slashed.

## Setting up Stash and Controller keys

Nominators are recommended to set up separate stash and controller accounts. Explanation and reasoning for generating distinct accounts for this purpose is elaborated in the keys section of the Wiki.

You can generate your stash and controller account via any of the recommended methods that are detailed on the account generation page.

Starting with runtime version v23 natively included in client version 0.8.23, payouts can go to any custom address. If you'd like to redirect payments to an account that is neither the controller nor the stash account, set one up. Note that it is extremely unsafe to set an exchange address as the recipient of the staking rewards.

## Using Clover-JS UI

### Step 1: Bond your tokens

On the[ Clover-JS UI](https://apps.clover.finance/#/explorer) navigate to the "Staking" tab \(within the "Network" menu\).

The "Staking Overview" subsection will show you all the active validators and their information - their identities, the amount of CLV that are staking for them, amount that is their own provided stake, how much they charge in commission, the era points they've earned in the current era, and the last block number that they produced. If you click on the chart button it will take you to the "Validator Stats" page for that validator that shows you more detailed and historical information about the validator's stake, rewards and slashes.

The "Account actions" subsection \([link](https://apps.clover.finance/#/explorer)\) allows you to stake and nominate.

The "Payouts" subsection \([link](https://apps.clover.finance/#/explorer)\) allows you to claim rewards from staking.

The "Targets" subsection \([link](https://apps.clover.finance/#/explorer)\) will help you estimate your earnings and this is where it's good to start picking favorites.

The "Waiting" subsection \([link](https://apps.clover.finance/#/explorer)\) lists all pending validators that are awaiting more nominations to enter the active validator set. Validators will stay in the waiting queue until they have enough ClV backing them \(as allocated through the Phragmén election mechanism\). It is possible validator can remain in the queue for a very long time if they never get enough backing.

The "Validator Stats" subsection \([link](https://apps.clover.finance/#/explorer)\) allows you to query a validator's stash address and see historical charts on era points, elected stake, rewards, and slashes.

Pick "Account actions", then click the "+ Nominator" button.

You will see a modal window that looks like the below: 

Select a "value bonded" that is **less** than the total amount of CLV you have, so you have some left over to pay transaction fees. Transaction fees are currently around 0.01 CLV, but they are dynamic based on a variety of factors including the load of recent blocks.

Also be mindful of the reaping threshold - the amount that must remain in an account lest it be burned. That amount is 1 CLV on Clover, so it's recommended to keep at least 1.5 CLV in your account to be on the safe side.

Choose whatever payment destination that makes sense to you. If you're unsure, you can choose "Stash account \(increase amount at stake\)" to simply accrue the rewards into the amount you're staking and earn compound interest.

### Step 2: Nominate a validator

You are now bonded. Being bonded means your tokens are locked and could be slashed if the validators you nominate misbehave. All bonded funds can now be distributed to up to 16 validators. Be careful about the validators you choose since you will be slashed if your validator commits an offence.

Click on "Nominate" on an account you've bonded and you will be presented with another popup asking you to select up to 16 validators. Although you may choose up to 16 validators, due to the Phragmén election algorithm your stake may be dispersed in different proportions to any subset or all of the validators your choose.

Select them, confirm the transaction, and you're done - you are now nominating. Your nominations will become active in the next era. Eras last twenty-four hours on Clover - depending on when you do this, your nominations may become active almost immediately, or you may have to wait almost the entire twenty-four hours before your nominations are active. You can chek how far along Clover is in the current era on the [Staking page](https://apps.clover.finance/#/explorer).

Assuming at least one of your nominations ends up in the active validator set, you will start to get rewards allocated to you. In order to claim them \(i.e., add them to your account\), you must manually claim them. See the Claiming Rewards section of the Staking wiki page for more details.

### Step 3: Stop nominating

At some point, you might decide to stop nominating one or more validators. You can always change who you're nominating, but you cannot withdraw your tokens unless you unbond them. Detailed instructions are available [here](https://app.gitbook.com/@clover-network/s/portal/maintain/nominator-guides/unbonding-and-rebonding/@drafts).

## Using Command-Line Interface \(CLI\)

Apart from using Clover-JS Apps to participate in staking, you can do all these things in CLI instead. The CLI approach allows you to interact with the Clover network without going to the Clover-JS Apps dashboard.

### Step 1: Install @clover/api-cli

We assume you have installed [NodeJS with npm](https://nodejs.org/). Run the following command to install the `@clover/api-cli` globally:

```text
npm install -g @clover/api-cli
```

### Step 2. Bond your clv

Executing the following command:

```text
clover-js-api --seed "MNEMONIC_PHRASE" tx.staking.bond CONTROLLER_ADDRESS NUMBER_OF_TOKENS REWARD_DESTINATION --ws WEBSOCKET_ENDPOINT
```

`CONTROLLER_ADDRESS`: An address you would like to bond to the stash account. Stash and Controller can be the same address but it is not recommended since it defeats the security of the two-account staking model.

`NUMBER_OF_TOKENS`: The number of CLV you would like to stake to the network.

> **Note**: CLV has ten decimal places and is always represented as an integer with zeroes at the end. So 1 CLV = 10,000,000,000 units.

`REWARD_DESTINATION`:

* `Staked` - Pay into the stash account, increasing the amount at stake accordingly.
* `Stash` - Pay into the stash account, not increasing the amount at stake.
* `Account` - Pay into a custom account, like so: `Account DMTHrNcmA8QbqRS4rBq8LXn8ipyczFoNMb1X4cY2WD9tdBX`.
* `Controller` - Pay into the controller account.

Example:

```text
clover-js-api --seed "xxxx xxxxx xxxx xxxxx" tx.staking.bond DMTHrNcmA8QbqRS4rBq8LXn8ipyczFoNMb1X4cY2WD9tdBX 1000000000000 Staked --ws wss://api.clover.finance
```

Result:

```text
...
...
    "status": {
      "InBlock": "0x0ed1ec0ba69564e8f98958d69f826adef895b5617366a32a3aa384290e98514e"
    }
```

You can verify the bonding state under the [Staking](https://apps.clover.finance/#/explorer) page on the Clover-JS Apps Dashboard.

### Step 3. Nominate a validator

To nominate a validator, you can execute the following command:

```text
clover-js-api --seed "MNEMONIC_PHRASE" tx.staking.nominate '["VALIDATOR_ADDRESS"]' --ws WS_ENDPOINT
```

```text
clover-js-api --seed "xxxx xxxxx xxxx xxxxx" tx.staking.nominate '["CmD9vaMYoiKe7HiFnfkftwvhKbxN9bhyjcDrfFRGbifJEG8","E457XaKbj2yTB2URy8N4UuzmyuFRkcdxYs67UvSgVr7HyFb"]' --ws wss://api.clover.finance
```

After a few seconds, you should see the hash of the transaction and if you would like to verify the nomination status, you can check that on the Clover-JS UI as well.

