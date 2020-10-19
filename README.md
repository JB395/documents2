<pre>
  QIP-26
  Layer: Blockchain Protocol
  Title: Reduce Block Spacing
  Author: Jackson Belove jb395official@gmail.com
  Comments-Summary: Proposing reducing block spacing to provide faster
  confirmations for transactions, including smart contract transactions.
  Reduce average block spacing from the current 128 seconds to 16 seconds.
  Adjust block subsidy, halvings, and other blockchain parameters to
  preserve previous characteristics.

  Comments-URI: https://github.com/qtumproject/qips/issues/26
  Status: Draft
  Created: 2020-10-18
  License: GPLv3
</pre>

***

## Abstract ##

This Qtum Improvement Proposal (QIP) describes changes in blockchain parameters and logic to reduce target block spacing (the average spacing between blocks). 

From launch, Qtum has used a target block spacing (a configuration parameter) of 128 seconds. Target and difficulty are adjusted for every block to maintain the average 128 seconds block spacing, with considerable variance due to the randomness needed for decentralized consensus.

This proposal would change parameters and perhaps other logic, to reduce the average block spacing to 16 seconds, or 32 seconds if 16 seconds proves too difficult. The consensus algorithm currently evaluates every UTXO at 16-second intervals, targeting a solution (publishing a block) after 8 intervals, or 128 seconds. A block could be published after the first interval (in 16 seconds) or occasionally, long after the target of 8 intervals.

New block headers and new blocks can be proposed continuously on the network, but consensus logic validates with a 16-second granularity, meaning blocks are validated and time-stamped with 16-second granularity according to the interval spacing.

## Motivation ##

The motivation for reducing block spacing is faster confirmation for value transfers and smart contract transactions, including DeFi applications, and optionally more uniform block spacings, and further reduction of the longest block spacings.

## Specification ##

### Preserve Blockchain Characteristics ###

The high-level design goals for reduced block spacing are:

* Maintain critical blockchain characteristics such as coin maturity time, issuance (block subsidy amount for block rewards), halving schedule, etc.
* Adjust block size to maintain TPS (Transactions Per Second) and max gas per block.
* Adjust the subsidy, the new QTUM that is created for each block as part of the block reward. This would likely be the current subsidy (4.0 QTUM) divided by “spacing improvement” (128 / 16 = 8), or 0.5 QTUM (for 16-second blocks). This keeps the overall inflation rate the same.
* With a faster cadence of intervals, nodes with slower processors, especially if they are staking large UTXO sets (> 1,000 UTXOs) may not be able to keep up and publish blocks.


### TPS and Block Size ###

Qtum blockchain currently has a theoretical (calculated) TPS (Transactions Per Second) of 70, and this QIP would preserve this value.

The current block size of 2 million bytes may be adjusted by current block size / “spacing improvement”. This would give a block size of 250,000. DGP (Distributed Governance Protocol) would maintain the same range of on-chain block size adjustment, e.g., the post QIP block size would remain the 2nd step up to 4,000,000 bytes (for 16-second blocks). 

## Block Spacing Distribution ##

The histogram of block spacings follows a curve controlled by the target adjustment algorithm giving the most spacings in the first interval, 2nd most in the second interval, etc. The standard histogram looks like this:

 

It may be possible to “groom” the use of early intervals, while maintaining the expected average block spacing, using various logic modifications, to have a smoother and more uniform distribution of spacings for the early intervals. Perhaps there could be a “hold off” to prohibit the use of the first interval, etc. This is a stretch goal for reduced block spacing. 

Another possible logic modification is to reduce the very long spacing blocks, shown here as the blocks > 580 seconds. This is not required by the QIP but is another stretch goal.

## Back-End Nodes ##

With the faster block cadence care should be taken with back end systems supporting explorers, APIs, and light wallets, that they are capable to support faster throughput.

## Orphan Blocks ##

Orphan blocks (simultaneous consensus solutions) that publish two new blocks on the network for the same block height, temporarily splitting the blockchain. Orphan blocks are in the 2 – 3% range currently for Qtum mainnet. Using Nakamoto Consensus (chain with most difficulty survives) one block becomes an orphan and is discarded. The solutions for reduced block spacing should consider ways to minimize any increase in the orphan block rate.

## Compatibility ##

Reducing block spacing requires block parameters and logic changes, the updated node, and other back end systems will not be backward compatible and will require a hard fork for implementation. It would be possible to run a separate test network in advance of deployment and follow the pattern of previous Qtum hard forks, i.e., activation at a preset block height on Testnet followed by activation on Mainnet.

## Nongoals ##

The following features are nongoals, and will not be provided in this QIP.

* Increase TPS or make any changes to Distributed Governance Protocol, which will continue in effect.
* Preserve hardware compatibility with all lower-powered nodes, especially if they are staking large UTXO sets. Some slower node platforms may be sunset.

## Acknowledgments ##

The author would like to thank the Qtum Chain Foundation leadership and core developer team for their strategy and design of reduced block spacing.

## References ##

1. Developer’s Guide http://book.qtum.site/en/
2. Qtum Documentation https://docs.qtum.site/en/
3. GitHub Qtum Core repository https://github.com/qtumproject/qtum

## Copyright ##

Qtum and this QIP are licensed under GNU General Public License v3.
