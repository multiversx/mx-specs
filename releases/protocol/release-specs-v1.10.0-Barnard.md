[comment]: <> (tags/v1.10.0)

# Contents

This document explains the contents of the release codenamed Spica. It is split in 2 sections:
- the **features** list containing detailed insights of the feature along with the external impact and the relevant
  pull requests list
- the **smaller features and fixes** area contains the one-pull request small features or fixes along with the
  external impact details

This documentation is relevant for the `rc/barnard` branch.

# Features

## 1. Governance updates [#6490](https://github.com/multiversx/mx-chain-go/pull/6490)

This pull request introduces several enhancements and refactors to the governance smart contract:
* It adds stricter voting period validations, introduces a new mechanism for tracking and clearing ended proposals and user votes, and implements support for a new user vote tracking structure (V2) with backward compatibility for V1.
* Additional improvements include enhanced gas management, configurable voting delay, and more precise handling of proposal closure and results computation.
* Some redundant or outdated methods (like the old user vote history view) are removed, and formatting/percentage calculations are improved for accuracy and clarity.

It improves the governance smart contract by:
* tightening voting rules, adding better tracking and cleanup for proposals and user votes
* supporting a new vote tracking format (V2) while keeping V1 compatibility.
* improves gas use, allows voting delays, and refines how proposals are closed and results calculated.
* Outdated methods are removed, and formatting and percentage handling are more accurate.

### Impact:

* Configuration changes:
  * There are two new enable epoch definitions for this feature, called `GovernanceDisableProposeEnableEpoch` and `GovernanceFixesEnableEpoch`
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#5857](https://github.com/multiversx/mx-chain-go/pull/5857) - Accept multiple delegateVoting
- [#5877](https://github.com/multiversx/mx-chain-go/pull/5877) - MaxVotingDelayPeriodInEpochs
- [#5872](https://github.com/multiversx/mx-chain-go/pull/5872) - Add gasUsage for changeConfig
- [#5922](https://github.com/multiversx/mx-chain-go/pull/5922) - Add clean method
- [#6496](https://github.com/multiversx/mx-chain-go/pull/6496) - Add cancel method before voting start
- [#6868](https://github.com/multiversx/mx-chain-go/pull/6868) - Float64 conversion issue fix

## 2. Delegation improvements v3 [#6605](https://github.com/multiversx/mx-chain-go/pull/6605)

This feature enables users to move funds from one staking provider to another once every 30 days. 

In the same time, staking providers will not be allowed to change the fee earlier than 30 days.

### Impact:

* Configuration changes:
  * There is a new enable epoch definition for this feature, called `DelegationImprovementsV3EnableEpoch`
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#6500](https://github.com/multiversx/mx-chain-go/pull/6500) - Claim rewards with call value should error
- [#6503](https://github.com/multiversx/mx-chain-go/pull/6503) - IncreaseFee with 30 epoch cooldown
- [#6509](https://github.com/multiversx/mx-chain-go/pull/6509) - Migrate from staking provider to staking provider
- [#6591](https://github.com/multiversx/mx-chain-go/pull/6591) - Integration tests migrate delegation

## 3. Accounts storage iterator [#6547](https://github.com/multiversx/mx-chain-go/pull/6547)

This feature enables retrieval of tokens from a data trie in a sequential order, with the possibility of continuing from a checkpoint if the data trie has lots of leaves, thus iterating thorough all the leaves in multiple accesses.

### Impact:

* No configuration changes
* No Node CLI arguments changes
* New node API endpoint was added, `/address/iterate-keys`

### Relevant PRs:
- [#6086](https://github.com/multiversx/mx-chain-go/pull/6086) - Accounts storage iterator
- [#6696](https://github.com/multiversx/mx-chain-go/pull/6696) - Integrate new account storage iterator

# Smaller features or fixes

- [#6264](https://github.com/multiversx/mx-chain-go/pull/6264) - Change penalise gas
- [#6511](https://github.com/multiversx/mx-chain-go/pull/6511) - Update compiler to go 1.23
- [#6932](https://github.com/multiversx/mx-chain-go/pull/6932) - VM reserved functions activation flag integration [integrates mx-chain-vm-go [#913](https://github.com/multiversx/mx-chain-vm-go/pull/913)]
- [#6793](https://github.com/multiversx/mx-chain-go/pull/6793) - Automatic activation of nodes disabled
- [#6548](https://github.com/multiversx/mx-chain-go/pull/6548) - New epoch start blockchain hooks
- [#6893](https://github.com/multiversx/mx-chain-go/pull/6893) - Add get ESDT token type vm hook [integrates mx-chain-vm-go [#909](https://github.com/multiversx/mx-chain-vm-go/pull/909)]
- [#6852](https://github.com/multiversx/mx-chain-go/pull/6852) - Integrate transfer and execute with return error [integrates mx-chain-vm-go [#907](https://github.com/multiversx/mx-chain-vm-go/pull/907)]
- [#6791](https://github.com/multiversx/mx-chain-go/pull/6791) - VM integration for backtransfers improvements [integrates mx-chain-vm-go [#899](https://github.com/multiversx/mx-chain-vm-go/pull/899)]
- [#6774](https://github.com/multiversx/mx-chain-go/pull/6774) - vm integration for get all transfers value hook, get code hash hook and internal errors masking [integrates mx-chain-vm-go [#895](https://github.com/multiversx/mx-chain-vm-go/pull/895), [#897](https://github.com/multiversx/mx-chain-vm-go/pull/897), [#898](https://github.com/multiversx/mx-chain-vm-go/pull/898)]
- [#6327](https://github.com/multiversx/mx-chain-go/pull/6327) - Check only dbs that are older than the given epoch
- [#6518](https://github.com/multiversx/mx-chain-go/pull/6518) - Optimize GitHub workflows
- [#6519](https://github.com/multiversx/mx-chain-go/pull/6519) - Optimize GitHub workflows
- [#6612](https://github.com/multiversx/mx-chain-go/pull/6612) - Heartbeats chain simulator
