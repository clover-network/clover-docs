# Staking on Ivy network

Clover Ivy network uses NPOS(Nominated Proof-of-Stake) as its consensus mechanism which is the same as [Polkadot](https://polkadot.network) uses. CLV users can participate the consensus as norminators. Norminators may support validator candiates by collatoring their CLV tokens and receive staking rewards.

This tutorial focuses on how to staking on Clover Ivy netowrk. The detailed information of how staking works could be found at the [Polkadot Staking wiki pages.](https://wiki.polkadot.network/docs/learn-staking)



## Setup Accounts

There are two accounts in Clover: `Stash` and `Controller` . It's recommended to create two separate accounts for `Stash` and `Controller`.

![Stash And Controller accounts.](<../.gitbook/assets/image (89).png>)

`Stash` account is the main account of holding your funds. It delegates some staking permission to the `Controller` account.

To grant the staking permissions to the `Controller` account, You need go to the `Staking/Account Actions` page. Click on the `Norminator` button.

![](<../.gitbook/assets/image (90).png>)

Then the `Setup up norminator` dialogs show up:

![Setup norminator dialog 1](<../.gitbook/assets/image (94).png>)

Select your `Stash` and `Controller` accounts and set the value bonded. The payment destination options controls where the staking reward goes. Click `next` button.

{% hint style="info" %}
Do keep some balances in your stash account(aka, don't bond all of your funds). You still need some funds to pay the transaction fees.
{% endhint %}

![Setup norminator dialog2](<../.gitbook/assets/image (93).png>)

You can select some validator candiates to norminate in the second page.&#x20;

{% hint style="warning" %}
Do only select the validators you trust! Picking candinates only on their current profitablitity could lead to reduced profits or even loss of funds!

Misbehaved validators will be `slashed` as well as norminators who norminiated them.
{% endhint %}

Confirm and send the transaction and wait for the transaction is included in the block chain. The account details will show in the `Account Details` page.

![Normation details](<../.gitbook/assets/image (95).png>)

More actions could be found by clicking on the dots at the right side of the account details:

![Account actions](<../.gitbook/assets/image (91).png>)

You can:

1. Bond more funds
2. Unbond funds
3. Change `Controller` account
4. Change reward destination
5. Set nominees

