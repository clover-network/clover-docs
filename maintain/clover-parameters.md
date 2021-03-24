---
description: 'from https://wiki.polkadot.network/docs/en/maintain-polkadot-parameters'
---

# Clover Parameters

Many of these parameter values can be updated via on-chain governance. If you require absolute certainty as to their values, it is recommended you directly check the constants by looking at the [chain state](https://apps.clover.finance/#/explorer) and/or [storage](https://apps.clover.finance/#/explorer).

## Periods of common actions and attributes

_NOTE: Clover runs 4x as fast as Clover, except Clover also has 6 second blocktimes. See Clover Parameters for more details on how Clover's parameters differ from Clover's._

* Slot: 6 seconds \*\(generally one block per slot, although see note below\)
* Epoch: 4 hours \(2,400 slots x 6 seconds\)
* Session: 4 hours \(Session and Epoch lengths are the same\)
* Era: 24 hours \(6 sessions per Era, 2,400 slots x 6 epochs x 6 seconds\)

| Clover | Time | Slots\* |
| :--- | :--- | :--- |
| Slot | 6 seconds | 1 |
| Epoch | 4 hours | 2,400 |
| Session | 4 hours | 2,400 |
| Era | 24 hours | 14,400 |

\*_A maximum of one block per slot can be in a canonical chain. However, occasionally a slot will be without a block in the chain. Thus, the times given are estimates. See Consensus for more details._

## Governance

| Democracy | Time | Slots | Description |
| :--- | :--- | :--- | :--- |
| Voting period | 28 days | 403,200 | How long the public can vote on a referendum. |
| Launch period | 28 days | 403,200 | How long the public can select which proposal to hold a referendum on, i.e., every week, the highest-weighted proposal will be selected to have a referendum. |
| Enactment period | 28 days | 403,200 | Time it takes for a successful referendum to be implemented on the network. |

| Council | Time | Slots | Description |
| :--- | :--- | :--- | :--- |
| Term duration | 7 days | 100,800 | The length of a council member's term until the next election round. |
| Voting period | 7 days | 100,800 | The council's voting period for motions. |

The Clover Council consists of up to 13 members and up to 20 runners up.

| Technical committee | Time | Slots | Description |
| :--- | :--- | :--- | :--- |
| Cool-off period | 7 days | 100,800 | The time a veto from the technical committee lasts before the proposal can be submitted again. |
| Emergency voting period | 3 hours | 1,800 | The voting period after the technical committee expedites voting. |

## Staking, Validating, and Nominating

| Clover | Time | Slots | Description |
| :--- | :--- | :--- | :--- |
| Term duration | 1 Day | 14,400 | The time for which a validator is in the set after being elected. Note, this duration can be shortened in the case that a validator misbehaves. |
| Nomination period | 1 Day | 14,400 | How often a new validator set is elected according to Phragmén's method. |
| Bonding duration | 28 days | 403,200 | How long until your funds will be transferrable after unbonding. Note that the bonding duration is defined in eras, not directly by slots. |
| Slash defer duration | 28 days | 403,200 | Prevents overslashing and validators "escaping" and getting their nominators slashed with no repercussions to themselves. Note that the bonding duration is defined in eras, not directly by slots. |

## Treasury

| Treasury | Time | Slots | Description |
| :--- | :--- | :--- | :--- |
| Periods between spends | 24 days | 345,600 | When the treasury can spend again after spending previously. |

Burn percentage is currently `1.00%`.

## Precision

CLV have 12 decimals of precision. In other words, 10 \*\* 10 \(10,000,000,000 or ten billion\) Plancks make up a CLV.

The denomination of CLV was changed from 12 decimals of precision at block \#1,248,328 in an event known as _Denomination Day_. See Redenomination for details.

