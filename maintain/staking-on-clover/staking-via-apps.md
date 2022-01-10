# Staking via Apps

## Create Accounts

You can go to [https://apps-ivy.clover.finance/](https://apps-ivy.clover.finance) to setup the accounts for staking. There are two accounts in Clover: `Stash` and `Controller` . It's recommended to create two separate accounts for `Stash` and `Controller`. And you can import your existing accounts as well.

![Stash And Controller accounts.](<../../.gitbook/assets/image (89) (1) (1) (1).png>)

`Stash` account is the main account of holding your funds. It delegates some staking permission to the `Controller` account.

## Set Accounts and CLV Bonded

To grant the staking permissions to the `Controller` account, You need go to the `Staking/Account Actions` page. Click on the `Nominator` button.

![](<../../.gitbook/assets/image (90) (1).png>)

Then the `Setup up nominator` dialogs show up:

![Setup nominator dialog 1](<../../.gitbook/assets/image (94) (1) (1) (1) (1).png>)

Select your `Stash` and `Controller` accounts and set the value bonded. The payment destination options controls where the staking reward goes. Click `next` button.

{% hint style="info" %}
Do keep some balances in your stash account(aka, don't bond all of your funds). You still need some funds to pay the transaction fees.
{% endhint %}

## Select Nominators

![Setup nominator dialog 2](<../../.gitbook/assets/image (93) (1) (1) (1).png>)

You can select some validator candidates to nominate in the second page.&#x20;

{% hint style="warning" %}
Do only select the validators you trust! Picking candidates only on their current profitability could lead to reduced profits or even loss of funds!

Misbehaved validators will be `slashed` as well as nominators who nominated them.
{% endhint %}

## Bond and Nominate

Confirm and send the transaction and wait for the transaction is included in the block chain. The account details will show in the `Account Details` page.

![Nomination details](<../../.gitbook/assets/image (95) (1) (1) (1).png>)

More actions could be found by clicking on the dots at the right side of the account details:

![Account actions](<../../.gitbook/assets/image (91) (1) (1).png>)

You can:

1. Bond more funds
2. Unbond funds
3. Change `Controller` account
4. Change reward destination
5. Set nominees
