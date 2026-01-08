[comment]: <> (tags/v2.0.0)

# Contents

This document explains the contents of the release codenamed Supernova. It is split in 2 sections:
- the **features** list containing detailed insights of the feature along with the external impact and the relevant
  pull requests list
- the **smaller features and fixes** area contains the one-pull request small features or fixes along with the
  external impact details

This documentation is relevant for the `tags/v2.0.0` tag release.

# Features

## 1. Sub-second Round [#7075](https://github.com/multiversx/mx-chain-go/pull/7075)

This feature implements millisecond-level round granularity, enabling sub-second block times for the Supernova upgrade. The key changes include:

* Chronology shift from seconds to milliseconds timing resolution
* Added `RoundsPerEpoch` to TOML config for activation epoch fetching
* Introduced round activation mechanism for the Supernova upgrade
* Modified round handler to include new round duration and start timestamp
* Process sync tracker now validates header timestamps relative to genesis time
* Updated API/outport drivers to support millisecond-based header timestamps

### Impact:

* Configuration changes:
  * `RoundActivations` now holds `SupernovaEnableRound`, which is the round when Supernova will be fully active
  * `economics.toml` was updated to reflect the new gas limits for transaction/miniblock/block
  * multiple config values were moved to new structures inside `config.toml` to have them epoch/round-based
* No Node CLI arguments changes
* API endpoints updated to support millisecond timestamps

### Relevant PRs:
- [#7374](https://github.com/multiversx/mx-chain-go/pull/7374) Feat sub second round 2
- [#7298](https://github.com/multiversx/mx-chain-go/pull/7298) Fix sub-second-round integration tests
- [#7205](https://github.com/multiversx/mx-chain-go/pull/7205) Sub-second round local testnet fixes
- [#7176](https://github.com/multiversx/mx-chain-go/pull/7176) Adapt integration tests sub second

## 2. Mempool Supernova [#7114](https://github.com/multiversx/mx-chain-go/pull/7114)

This feature implements core enhancements to the transaction pool for the Supernova upgrade:

* Notification callbacks (on block proposed, on block executed) to keep the pool informed about block sequencing
* Tracking mechanisms for proposed blocks, critical for transaction selection during the Supernova phase
* Additional transactions removal mechanism (auto-clean) to evict non-executable transactions deterministically
* New API endpoints including virtual nonces and transaction selection simulation
* SelectionTracker component to manage proposed/executed block sequences
* Virtual selection session infrastructure deriving state from tracked blocks

### Impact:

* No major configuration changes
* No Node CLI arguments changes
* New Node HTTP API endpoints:
  * `/pool/:address/virtual-nonce` - Virtual nonce endpoint for querying account nonces considering pending proposed blocks
  * `/pool/simulate-selection` - Selection simulation endpoint for previewing transaction selection outcomes

### Relevant PRs:
- [#7329](https://github.com/multiversx/mx-chain-go/pull/7329) Feat mempool supernova part2
- [#7154](https://github.com/multiversx/mx-chain-go/pull/7154) Move txcache into supernova
- [#7093](https://github.com/multiversx/mx-chain-go/pull/7093) Integration tests mempool
- [#7024](https://github.com/multiversx/mx-chain-go/pull/7024) Txpool autoclean
- [#7201](https://github.com/multiversx/mx-chain-go/pull/7201) Txpool autoclean updates
- [#7230](https://github.com/multiversx/mx-chain-go/pull/7230) Select transactions endpoint
- [#7241](https://github.com/multiversx/mx-chain-go/pull/7241) Get virtual nonce endpoint
- [#7254](https://github.com/multiversx/mx-chain-go/pull/7254) Virtual nonce endpoint fixes
- [#7311](https://github.com/multiversx/mx-chain-go/pull/7311) Txpool metrics
- [#7380](https://github.com/multiversx/mx-chain-go/pull/7380) Txpool context
- [#7493](https://github.com/multiversx/mx-chain-go/pull/7493) Integration tests mempool

## 3. Supernova Economics [#7167](https://github.com/multiversx/mx-chain-go/pull/7167)

This feature modifies how inflation rates are calculated, transitioning from round-based to timestamp-based methodology following Supernova activation:

* Inflation calculation transitions from round-based to timestamp-based methodology
* Backward compatibility maintained for pre-Supernova inflation computations
* Support for time-based computation after the SupernovaFlag activation point

### Impact:

* No major configuration changes
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#7144](https://github.com/multiversx/mx-chain-go/pull/7144) Economics transition rewards tests
- [#7146](https://github.com/multiversx/mx-chain-go/pull/7146) Arithmetic epoch provider transition
- [#7111](https://github.com/multiversx/mx-chain-go/pull/7111) Economics transition inflation rate

## 4. Async Execution [#7055](https://github.com/multiversx/mx-chain-go/pull/7055)

This feature implements asynchronous block execution capabilities for the Supernova upgrade, enabling consensus being done in parallel with blocks processing.

* Block proposal before execution
* Verification of the proposal logic change
* Component for estimating when an execution result can be included on the proposed block
* Processing queue
* Tracking of execution results and notifying the txpool of new selection context
* Selection of transactions from txpool with adaptive bandwidth

### Impact:

* Configuration changes:
  * `economics.toml` now holds `BlockCapacityOverestimationFactor`, that specifies how much a block capacity can be overestimated
  * new `RatingStepsByEpoch` configs
  * moved more constants to `config.toml` round-based structures
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#7462](https://github.com/multiversx/mx-chain-go/pull/7462) Chain simulator async execution
- [#7539](https://github.com/multiversx/mx-chain-go/pull/7539) Fixes async exec
- [#7560](https://github.com/multiversx/mx-chain-go/pull/7560) Fixes outport block async
- [#7585](https://github.com/multiversx/mx-chain-go/pull/7585) Fix Restore After Process Fail
- [#7586](https://github.com/multiversx/mx-chain-go/pull/7586) Fix Missing Tx At Bootstrap
- [#7583](https://github.com/multiversx/mx-chain-go/pull/7583) Fix Round Canceled When Header Received Early
- [#7581](https://github.com/multiversx/mx-chain-go/pull/7581) Fix Multiple Headers Not Being Referenced
- [#7582](https://github.com/multiversx/mx-chain-go/pull/7582) Fix Missing Txs At Bootstrap
- [#7564](https://github.com/multiversx/mx-chain-go/pull/7564) Fix Meta Bootstrap Sync
- [#7534](https://github.com/multiversx/mx-chain-go/pull/7534) Termui Add Exec Results Metrics
- [#7579](https://github.com/multiversx/mx-chain-go/pull/7579) Fixes Synchronized State
- [#7580](https://github.com/multiversx/mx-chain-go/pull/7580) Fix Gas Consumption For Cross
- [#7560](https://github.com/multiversx/mx-chain-go/pull/7560) Fixes Outport Block Async
- [#7578](https://github.com/multiversx/mx-chain-go/pull/7578) Fix Fully Referenced Metablock
- [#7577](https://github.com/multiversx/mx-chain-go/pull/7577) Fix Processed Mbs Tracking
- [#7556](https://github.com/multiversx/mx-chain-go/pull/7556) Late Broardcast Header And Proof
- [#7572](https://github.com/multiversx/mx-chain-go/pull/7572) Fix New Test
- [#7575](https://github.com/multiversx/mx-chain-go/pull/7575) Fix Gas Consumption Component
- [#7563](https://github.com/multiversx/mx-chain-go/pull/7563) Fix Failing Mempool Test
- [#7568](https://github.com/multiversx/mx-chain-go/pull/7568) Fixes Transaction Mismatch
- [#7539](https://github.com/multiversx/mx-chain-go/pull/7539) Fixes Async Exec
- [#7569](https://github.com/multiversx/mx-chain-go/pull/7569) Fix Test
- [#7567](https://github.com/multiversx/mx-chain-go/pull/7567) Fix Pop
- [#7559](https://github.com/multiversx/mx-chain-go/pull/7559) Fix Processed Mbs Tracking On Execution
- [#7562](https://github.com/multiversx/mx-chain-go/pull/7562) Fix Supernova Get Receipts Storer
- [#7561](https://github.com/multiversx/mx-chain-go/pull/7561) MX 17392 Configs By Round
- [#7558](https://github.com/multiversx/mx-chain-go/pull/7558) Fix Headers Cleanup For Self
- [#7546](https://github.com/multiversx/mx-chain-go/pull/7546) Fix Consumed Balance Check On Selection
- [#7462](https://github.com/multiversx/mx-chain-go/pull/7462) Chain Simulator Async Execution
- [#7552](https://github.com/multiversx/mx-chain-go/pull/7552) Metrics Update And New
- [#7532](https://github.com/multiversx/mx-chain-go/pull/7532) Leap Round Epoch Start Fix
- [#7549](https://github.com/multiversx/mx-chain-go/pull/7549) Fix Fees Metric
- [#7545](https://github.com/multiversx/mx-chain-go/pull/7545) Save Proposed Txs To Pool On Bootstrap
- [#7548](https://github.com/multiversx/mx-chain-go/pull/7548) Fix Bootstrap Meta
- [#7551](https://github.com/multiversx/mx-chain-go/pull/7551) MX 17389 Fix Bootstrap Trigger 2
- [#7550](https://github.com/multiversx/mx-chain-go/pull/7550) MX 17389 Fix Bootstrap Trigger
- [#7544](https://github.com/multiversx/mx-chain-go/pull/7544) MX 17338 Fix Unstake Nodes 2
- [#7542](https://github.com/multiversx/mx-chain-go/pull/7542) Fix Bootstrap With Txs
- [#7541](https://github.com/multiversx/mx-chain-go/pull/7541) Fix Blocks Queue
- [#7540](https://github.com/multiversx/mx-chain-go/pull/7540) Fix Selection Block Nonce
- [#7533](https://github.com/multiversx/mx-chain-go/pull/7533) Fixes Context
- [#7531](https://github.com/multiversx/mx-chain-go/pull/7531) Add Shard Chain Trigger Registry v3
- [#7529](https://github.com/multiversx/mx-chain-go/pull/7529) Fix Check Validity Tx Values
- [#7528](https://github.com/multiversx/mx-chain-go/pull/7528) Fix Cleanup Mbs
- [#7526](https://github.com/multiversx/mx-chain-go/pull/7526) Cache Execution Order Fixes Outport
- [#7521](https://github.com/multiversx/mx-chain-go/pull/7521) Get From Storage Shard Data Creator
- [#7522](https://github.com/multiversx/mx-chain-go/pull/7522) Fix Sync Reprocess On Fail
- [#7524](https://github.com/multiversx/mx-chain-go/pull/7524) Update Cleanup Pools
- [#7519](https://github.com/multiversx/mx-chain-go/pull/7519) 600m Gas Tx
- [#7523](https://github.com/multiversx/mx-chain-go/pull/7523) Fix Bootstrap Validator Stats Root Hash
- [#7520](https://github.com/multiversx/mx-chain-go/pull/7520) Fix Tx Execution Order Supernova
- [#7514](https://github.com/multiversx/mx-chain-go/pull/7514) Fix Boostrap Meta Restart
- [#7502](https://github.com/multiversx/mx-chain-go/pull/7502) MX 17378 Force Ntp Resync If Desynced
- [#7518](https://github.com/multiversx/mx-chain-go/pull/7518) Save Peer Info To Storage
- [#7503](https://github.com/multiversx/mx-chain-go/pull/7503) Fix Blocks Queue Resuming After A Pause
- [#7517](https://github.com/multiversx/mx-chain-go/pull/7517) Duplicated Log Data
- [#7493](https://github.com/multiversx/mx-chain-go/pull/7493) Integration Tests Mempool 28 Nov
- [#7501](https://github.com/multiversx/mx-chain-go/pull/7501) Fix Root Hash Mismatch Leader Bootstrap
- [#7516](https://github.com/multiversx/mx-chain-go/pull/7516) Get Header Type Update
- [#7511](https://github.com/multiversx/mx-chain-go/pull/7511) Fix Log Data Serialization
- [#7515](https://github.com/multiversx/mx-chain-go/pull/7515) Verify Created Mbs Sanity Fix
- [#7504](https://github.com/multiversx/mx-chain-go/pull/7504) Execution Results Handling Fixes
- [#7507](https://github.com/multiversx/mx-chain-go/pull/7507) Execution Results Handling Fixes Unit Tests
- [#7494](https://github.com/multiversx/mx-chain-go/pull/7494) Fix Boostrap From Storage Context
- [#7500](https://github.com/multiversx/mx-chain-go/pull/7500) Fix Cross Shard Mbs Inclusion
- [#7506](https://github.com/multiversx/mx-chain-go/pull/7506) Unit Tests 04 Dec
- [#7491](https://github.com/multiversx/mx-chain-go/pull/7491) Avoid Setbusy On Commit
- [#7485](https://github.com/multiversx/mx-chain-go/pull/7485) Increase Coverage 26 Nov
- [#7492](https://github.com/multiversx/mx-chain-go/pull/7492) MX 17370 Trigger Registry Load Save
- [#7481](https://github.com/multiversx/mx-chain-go/pull/7481) Fix Missing Shard Header
- [#7499](https://github.com/multiversx/mx-chain-go/pull/7499) Unit Tests Avoid Setbusy On Commit 02 Dec
- [#7495](https://github.com/multiversx/mx-chain-go/pull/7495) Fix Counting Metablocks In Epoch
- [#7490](https://github.com/multiversx/mx-chain-go/pull/7490) Meta Snapshot Fix
- [#7474](https://github.com/multiversx/mx-chain-go/pull/7474) Remove Todos
- [#7487](https://github.com/multiversx/mx-chain-go/pull/7487) Supernova Fixes 5
- [#7489](https://github.com/multiversx/mx-chain-go/pull/7489) Fix Miniblock Cast Error
- [#7486](https://github.com/multiversx/mx-chain-go/pull/7486) Fix Meta Unmarshalling
- [#7479](https://github.com/multiversx/mx-chain-go/pull/7479) Display Exec Results
- [#7482](https://github.com/multiversx/mx-chain-go/pull/7482) Fixes Epoch Transition
- [#7475](https://github.com/multiversx/mx-chain-go/pull/7475) Supernova Fixes 4
- [#7473](https://github.com/multiversx/mx-chain-go/pull/7473) Fix Results Tracker Last Notarized
- [#7478](https://github.com/multiversx/mx-chain-go/pull/7478) Fix Marshall Miniblocks Slice
- [#7472](https://github.com/multiversx/mx-chain-go/pull/7472) Propagate Root Hash Mismatch
- [#7465](https://github.com/multiversx/mx-chain-go/pull/7465) Fix Miniblocks Marshall Proto
- [#7470](https://github.com/multiversx/mx-chain-go/pull/7470) MX 17339 Fix Update Peer State
- [#7471](https://github.com/multiversx/mx-chain-go/pull/7471) Supernova Fixes 3
- [#7460](https://github.com/multiversx/mx-chain-go/pull/7460) Meta Process Proposal Unit Tests 24 Nov
- [#7457](https://github.com/multiversx/mx-chain-go/pull/7457) Meta Process Proposal Unit Tests 21 Nov
- [#7468](https://github.com/multiversx/mx-chain-go/pull/7468) Update Gas Config
- [#7469](https://github.com/multiversx/mx-chain-go/pull/7469) Sync Trie Integration Tests Fix
- [#7464](https://github.com/multiversx/mx-chain-go/pull/7464) Supernova Fixes p2
- [#7463](https://github.com/multiversx/mx-chain-go/pull/7463) Fix Tx Pool Selection Nonce
- [#7459](https://github.com/multiversx/mx-chain-go/pull/7459) Fix Prev Header Last Execution Result
- [#7458](https://github.com/multiversx/mx-chain-go/pull/7458) Supernova Fixes
- [#7435](https://github.com/multiversx/mx-chain-go/pull/7435) Meta Process Proposal
- [#7456](https://github.com/multiversx/mx-chain-go/pull/7456) Fix Epoch Start Bootstrap Shard Data Roothash
- [#7455](https://github.com/multiversx/mx-chain-go/pull/7455) Sync Meta Header Interface
- [#7450](https://github.com/multiversx/mx-chain-go/pull/7450) Meta Process Proposal Unit Tests 20 Nov
- [#7411](https://github.com/multiversx/mx-chain-go/pull/7411) Update Outport Driver
- [#7451](https://github.com/multiversx/mx-chain-go/pull/7451) Get All Miniblocks Body From Pool
- [#7453](https://github.com/multiversx/mx-chain-go/pull/7453) Fixes Meta Processing
- [#7454](https://github.com/multiversx/mx-chain-go/pull/7454) Fix Tx Pool Header On Executed
- [#7452](https://github.com/multiversx/mx-chain-go/pull/7452) Gas Tracker Handler
- [#7442](https://github.com/multiversx/mx-chain-go/pull/7442) Error Logs Fixes 19 Nov
- [#7448](https://github.com/multiversx/mx-chain-go/pull/7448) Adapt Metablock Interface v3
- [#7446](https://github.com/multiversx/mx-chain-go/pull/7446) MX 17332 Fix Sync Peer Mbs
- [#7444](https://github.com/multiversx/mx-chain-go/pull/7444) MX 17331 Get Last Executed Bloc
- [#7447](https://github.com/multiversx/mx-chain-go/pull/7447) Pruning Storer Headers v3
- [#7443](https://github.com/multiversx/mx-chain-go/pull/7443) Meta Epoch Change Trigger Update
- [#7439](https://github.com/multiversx/mx-chain-go/pull/7439) MX 17325 Meta Trigger Set Processed Split
- [#7441](https://github.com/multiversx/mx-chain-go/pull/7441) Add Last Execution Result To Chain Handler
- [#7438](https://github.com/multiversx/mx-chain-go/pull/7438) Meta Collect Execution Results
- [#7437](https://github.com/multiversx/mx-chain-go/pull/7437) Verify Epoch Start Data
- [#7440](https://github.com/multiversx/mx-chain-go/pull/7440) Meta Process Proposal Unit Tests 17 Nov
- [#7434](https://github.com/multiversx/mx-chain-go/pull/7434) Integration Tests Refactoring
- [#7429](https://github.com/multiversx/mx-chain-go/pull/7429) Integration Tests Exports
- [#7436](https://github.com/multiversx/mx-chain-go/pull/7436) MX 17324 Meta Process Proposal Fixes
- [#7430](https://github.com/multiversx/mx-chain-go/pull/7430) Save Txs From Cache To Storage
- [#7432](https://github.com/multiversx/mx-chain-go/pull/7432) Meta Processor Data To Broacast
- [#7433](https://github.com/multiversx/mx-chain-go/pull/7433) Fix Testscdeploy
- [#7431](https://github.com/multiversx/mx-chain-go/pull/7431) MX 17318 Commit Bloc v3 Get Headers
- [#7420](https://github.com/multiversx/mx-chain-go/pull/7420) Gas Tracker Optimization
- [#7422](https://github.com/multiversx/mx-chain-go/pull/7422) Fix Integration Tests
- [#7428](https://github.com/multiversx/mx-chain-go/pull/7428) Log Roothash For Cleanup
- [#7416](https://github.com/multiversx/mx-chain-go/pull/7416) MX 17307 Compute End Of Epoch Economics v3
- [#7413](https://github.com/multiversx/mx-chain-go/pull/7413) Consensus Integration
- [#7427](https://github.com/multiversx/mx-chain-go/pull/7427) Broadcast Shard Info Proposed Handlers
- [#7426](https://github.com/multiversx/mx-chain-go/pull/7426) Restart Execution On Failure
- [#7404](https://github.com/multiversx/mx-chain-go/pull/7404) Ratings Data Backwards Compatible Fix
- [#7417](https://github.com/multiversx/mx-chain-go/pull/7417) Adapt State Pruning And Commit For headerV3
- [#7423](https://github.com/multiversx/mx-chain-go/pull/7423) Fix Missing Headers Async Test
- [#7402](https://github.com/multiversx/mx-chain-go/pull/7402) Execution Manager
- [#7407](https://github.com/multiversx/mx-chain-go/pull/7407) MX 17302 Get Mb Headers With Dest Processed
- [#7414](https://github.com/multiversx/mx-chain-go/pull/7414) Request Missing Headers v3
- [#7394](https://github.com/multiversx/mx-chain-go/pull/7394) Meta Verify Block Proposal
- [#7421](https://github.com/multiversx/mx-chain-go/pull/7421) Fix Cleanup Root Hash
- [#7400](https://github.com/multiversx/mx-chain-go/pull/7400) Interceptor Validation New Meta Headers
- [#7418](https://github.com/multiversx/mx-chain-go/pull/7418) Meta Verify Block Proposal Unit Tests 11nov
- [#7410](https://github.com/multiversx/mx-chain-go/pull/7410) Meta Verify Block Proposal Unit Tests 10nov
- [#7408](https://github.com/multiversx/mx-chain-go/pull/7408) Meta Block v3 Block Info Context
- [#7409](https://github.com/multiversx/mx-chain-go/pull/7409) Fix Staking Integration Tests
- [#7406](https://github.com/multiversx/mx-chain-go/pull/7406) Empty Txs On Gas Consumption
- [#7403](https://github.com/multiversx/mx-chain-go/pull/7403) Fix Integration Tests Round Duration 2
- [#7401](https://github.com/multiversx/mx-chain-go/pull/7401) Fix Integration Tests Round Duration
- [#7397](https://github.com/multiversx/mx-chain-go/pull/7397) MX 17300 Verify Meta Block Proposal Extra Checks
- [#7396](https://github.com/multiversx/mx-chain-go/pull/7396) Unit Tests Meta Verify Block Proposal
- [#7380](https://github.com/multiversx/mx-chain-go/pull/7380) Txpool Context
- [#7387](https://github.com/multiversx/mx-chain-go/pull/7387) MX 17296 Sync Missing Shard Headers By Meta
- [#7389](https://github.com/multiversx/mx-chain-go/pull/7389) Interceptor Validation New Headers
- [#7395](https://github.com/multiversx/mx-chain-go/pull/7395) Chain Handler Root Hash
- [#7378](https://github.com/multiversx/mx-chain-go/pull/7378) Chain Simulator New Parameters
- [#7399](https://github.com/multiversx/mx-chain-go/pull/7399) Race Fixes
- [#7398](https://github.com/multiversx/mx-chain-go/pull/7398) Fix Maximum Selection Duration
- [#7391](https://github.com/multiversx/mx-chain-go/pull/7391) Save Receipts Commit Block Adjusting
- [#7384](https://github.com/multiversx/mx-chain-go/pull/7384) Fix Ratings Computation
- [#7393](https://github.com/multiversx/mx-chain-go/pull/7393) Num Pending On Shard Data Proposal
- [#7354](https://github.com/multiversx/mx-chain-go/pull/7354) Supernova Meta Processing
- [#7369](https://github.com/multiversx/mx-chain-go/pull/7369) Create Shardinfo Testing
- [#7390](https://github.com/multiversx/mx-chain-go/pull/7390) Rollback Current Block 2
- [#7385](https://github.com/multiversx/mx-chain-go/pull/7385) Revert Esdt Supply Based On Execution Results
- [#7388](https://github.com/multiversx/mx-chain-go/pull/7388) Shard Data Proposal Integration
- [#7364](https://github.com/multiversx/mx-chain-go/pull/7364) Pendingminiblocks For hv3
- [#7356](https://github.com/multiversx/mx-chain-go/pull/7356) Chain Handler Last Execution Info
- [#7355](https://github.com/multiversx/mx-chain-go/pull/7355) Revert Current Block
- [#7337](https://github.com/multiversx/mx-chain-go/pull/7337) Bootstrap Block v3
- [#7383](https://github.com/multiversx/mx-chain-go/pull/7383) Extra Log Rating
- [#7382](https://github.com/multiversx/mx-chain-go/pull/7382) Fix Integration Test Supernova
- [#7357](https://github.com/multiversx/mx-chain-go/pull/7357) Revert Incoming Miniblocks
- [#7379](https://github.com/multiversx/mx-chain-go/pull/7379) Prepare For Legacy Sync
- [#7286](https://github.com/multiversx/mx-chain-go/pull/7286) Adapt Old Gas Tracker
- [#7371](https://github.com/multiversx/mx-chain-go/pull/7371) Unit Tests Update Db Lookup Ext Supernova
- [#7365](https://github.com/multiversx/mx-chain-go/pull/7365) Update Db Lookup Ext Supernova
- [#7376](https://github.com/multiversx/mx-chain-go/pull/7376) Fix Genesis Notarization
- [#7358](https://github.com/multiversx/mx-chain-go/pull/7358) Fix Custom Configs Chain Simulator
- [#7285](https://github.com/multiversx/mx-chain-go/pull/7285) Supernova Unbond Period
- [#7349](https://github.com/multiversx/mx-chain-go/pull/7349) Cleanup Pools Header v3
- [#7129](https://github.com/multiversx/mx-chain-go/pull/7129) Fix Redundancy Comment
- [#7372](https://github.com/multiversx/mx-chain-go/pull/7372) Avoid Duplicates
- [#7350](https://github.com/multiversx/mx-chain-go/pull/7350) Shardblockproposal Testing
- [#7368](https://github.com/multiversx/mx-chain-go/pull/7368) Fix Jailed Nodes Supernova
- [#7327](https://github.com/multiversx/mx-chain-go/pull/7327) Supernova Tx Pool Commit Integration
- [#7363](https://github.com/multiversx/mx-chain-go/pull/7363) Fix Mutex Unlock
- [#7348](https://github.com/multiversx/mx-chain-go/pull/7348) Recreate Trie If Needed
- [#7352](https://github.com/multiversx/mx-chain-go/pull/7352) Fix Eviction Consistency
- [#7344](https://github.com/multiversx/mx-chain-go/pull/7344) Todos Solved
- [#7347](https://github.com/multiversx/mx-chain-go/pull/7347) Proper Accounts Adapter For Proposal
- [#7353](https://github.com/multiversx/mx-chain-go/pull/7353) Fix Chain Simulator Genesis Time Init
- [#7351](https://github.com/multiversx/mx-chain-go/pull/7351) Time Duration Fix Manual Round Handler
- [#7332](https://github.com/multiversx/mx-chain-go/pull/7332) Check Roothash 10 10
- [#7325](https://github.com/multiversx/mx-chain-go/pull/7325) Remove Blocks On Derive
- [#7339](https://github.com/multiversx/mx-chain-go/pull/7339) Supernova Timestamp For Round
- [#7304](https://github.com/multiversx/mx-chain-go/pull/7304) Sync Block v3
- [#7320](https://github.com/multiversx/mx-chain-go/pull/7320) Broadcast Executed Mbs
- [#7338](https://github.com/multiversx/mx-chain-go/pull/7338) Fix Genesis Time Mismatch In Chain Simulator
- [#7227](https://github.com/multiversx/mx-chain-go/pull/7227) Termui New Metrics
- [#7335](https://github.com/multiversx/mx-chain-go/pull/7335) Process Block Proposal Tests
- [#7336](https://github.com/multiversx/mx-chain-go/pull/7336) Start Time As Unix Seconds
- [#7309](https://github.com/multiversx/mx-chain-go/pull/7309) Reset Selection Tracker
- [#7312](https://github.com/multiversx/mx-chain-go/pull/7312) Cache Executed Mbs
- [#7326](https://github.com/multiversx/mx-chain-go/pull/7326) Update Gas Limits In Case Of Shard Is Stuck
- [#7333](https://github.com/multiversx/mx-chain-go/pull/7333) Fix Integration Tests Supernova Genesis Time
- [#7305](https://github.com/multiversx/mx-chain-go/pull/7305) Fix Process Compute Rating
- [#7231](https://github.com/multiversx/mx-chain-go/pull/7231) Unique Chunks Processor
- [#7228](https://github.com/multiversx/mx-chain-go/pull/7228) Termui Update Interval
- [#7287](https://github.com/multiversx/mx-chain-go/pull/7287) Process Block Proposal
- [#7300](https://github.com/multiversx/mx-chain-go/pull/7300) Use Global Account Breadcrumbs
- [#7311](https://github.com/multiversx/mx-chain-go/pull/7311) Txpool Metrics
- [#7321](https://github.com/multiversx/mx-chain-go/pull/7321) Fix Chainsimulator Config
- [#7299](https://github.com/multiversx/mx-chain-go/pull/7299) Create Header New Version
- [#7315](https://github.com/multiversx/mx-chain-go/pull/7315) Fix Verifygaslimit
- [#7279](https://github.com/multiversx/mx-chain-go/pull/7279) Integrate Gas Consumption Into Proposal Verification
- [#7314](https://github.com/multiversx/mx-chain-go/pull/7314) Fix Race On Notify All Test
- [#7307](https://github.com/multiversx/mx-chain-go/pull/7307) Integration Tests Forks
- [#7308](https://github.com/multiversx/mx-chain-go/pull/7308) Bug Fix Txpool
- [#7310](https://github.com/multiversx/mx-chain-go/pull/7310) Fix/ci False Positive On Test Failures
- [#7294](https://github.com/multiversx/mx-chain-go/pull/7294) Integrate Global Account Breadcrumb
- [#7298](https://github.com/multiversx/mx-chain-go/pull/7298) Fix Sub Second Round Integration Tests
- [#7290](https://github.com/multiversx/mx-chain-go/pull/7290) Select Txs Ppu Info
- [#7288](https://github.com/multiversx/mx-chain-go/pull/7288) Keep Breadcrumbs Compiled
- [#7284](https://github.com/multiversx/mx-chain-go/pull/7284) Check Tracked Tx Fixes
- [#7214](https://github.com/multiversx/mx-chain-go/pull/7214) Api Block Updates Async Execution
- [#7205](https://github.com/multiversx/mx-chain-go/pull/7205) Sub Second Round Local Testnet Fixes
- [#7278](https://github.com/multiversx/mx-chain-go/pull/7278) Check Tracked Tx
- [#7280](https://github.com/multiversx/mx-chain-go/pull/7280) Optimize Tx Check 09 24
- [#7266](https://github.com/multiversx/mx-chain-go/pull/7266) Verify Block Proposal
- [#7274](https://github.com/multiversx/mx-chain-go/pull/7274) Create And Verify Proposal Integration Test
- [#7249](https://github.com/multiversx/mx-chain-go/pull/7249) Supernova Gas Limits Updates
- [#7272](https://github.com/multiversx/mx-chain-go/pull/7272) Fixed Failing Integration Tests
- [#7273](https://github.com/multiversx/mx-chain-go/pull/7273) Verify Block Unit Tests
- [#7219](https://github.com/multiversx/mx-chain-go/pull/7219) Consensus Time Limits Improvements
- [#7209](https://github.com/multiversx/mx-chain-go/pull/7209) Update Ntp Sync Out Of Bounds Checks
- [#7199](https://github.com/multiversx/mx-chain-go/pull/7199) Epoch Start Trigger Optimizations
- [#7197](https://github.com/multiversx/mx-chain-go/pull/7197) Storer Optimizations Epoch Change
- [#7243](https://github.com/multiversx/mx-chain-go/pull/7243) Integrate New Gas Component
- [#7238](https://github.com/multiversx/mx-chain-go/pull/7238) Create Block Proposal
- [#7269](https://github.com/multiversx/mx-chain-go/pull/7269) Improve Selection Endpoint
- [#7271](https://github.com/multiversx/mx-chain-go/pull/7271) Failing Tests Fix
- [#7268](https://github.com/multiversx/mx-chain-go/pull/7268) Inclusion Estimator Update
- [#7267](https://github.com/multiversx/mx-chain-go/pull/7267) Create Block Proposal Shard Coverage
- [#7265](https://github.com/multiversx/mx-chain-go/pull/7265) Fix Account Breadcrumb Constructor
- [#7264](https://github.com/multiversx/mx-chain-go/pull/7264) Fix Failing Integration Test
- [#7261](https://github.com/multiversx/mx-chain-go/pull/7261) Txpool Improvements 16 09
- [#7254](https://github.com/multiversx/mx-chain-go/pull/7254) Virtual Nonce Endpoint Fixes
- [#7248](https://github.com/multiversx/mx-chain-go/pull/7248) Autoclean Updates
- [#7263](https://github.com/multiversx/mx-chain-go/pull/7263) Create Block Proposal Fix Some Tests
- [#7253](https://github.com/multiversx/mx-chain-go/pull/7253) Txpool Improvements 09 15
- [#7245](https://github.com/multiversx/mx-chain-go/pull/7245) Txpool Improvements 09 12
- [#7241](https://github.com/multiversx/mx-chain-go/pull/7241) Get Virtual Nonce Endpoint
- [#7251](https://github.com/multiversx/mx-chain-go/pull/7251) Get Timestamp For Round
- [#7246](https://github.com/multiversx/mx-chain-go/pull/7246) Supernova Into Txpool 09 12
- [#7230](https://github.com/multiversx/mx-chain-go/pull/7230) Select Transactions Endpoint
- [#7237](https://github.com/multiversx/mx-chain-go/pull/7237) Propagate Select Transactions Error
- [#7194](https://github.com/multiversx/mx-chain-go/pull/7194) Execution Result Inclusion Estimator
- [#7224](https://github.com/multiversx/mx-chain-go/pull/7224) Unit Test Block Data Request
- [#7229](https://github.com/multiversx/mx-chain-go/pull/7229) Missing Data Requester Update
- [#7222](https://github.com/multiversx/mx-chain-go/pull/7222) Tx Coordinator Requests
- [#7225](https://github.com/multiversx/mx-chain-go/pull/7225) Autoclean Integration 09 02
- [#7223](https://github.com/multiversx/mx-chain-go/pull/7223) Fixes And Refactoring
- [#7226](https://github.com/multiversx/mx-chain-go/pull/7226) Remove Diagnose Counters
- [#7215](https://github.com/multiversx/mx-chain-go/pull/7215) Replace Duplicated Nonce
- [#7221](https://github.com/multiversx/mx-chain-go/pull/7221) Integration 08 29
- [#7212](https://github.com/multiversx/mx-chain-go/pull/7212) Add Max Tracked Blocks
- [#7220](https://github.com/multiversx/mx-chain-go/pull/7220) Async Processing Integration 1
- [#7217](https://github.com/multiversx/mx-chain-go/pull/7217) Integration 08 28
- [#7216](https://github.com/multiversx/mx-chain-go/pull/7216) Fix Gh Ci Integration Tests
- [#7218](https://github.com/multiversx/mx-chain-go/pull/7218) Fix Zero Check
- [#7206](https://github.com/multiversx/mx-chain-go/pull/7206) Accounts Provider 08 25
- [#7201](https://github.com/multiversx/mx-chain-go/pull/7201) Txpool Autoclean Updates
- [#7024](https://github.com/multiversx/mx-chain-go/pull/7024) Txpool Autoclean
- [#7210](https://github.com/multiversx/mx-chain-go/pull/7210) Tracked Blocks Slice Into Map
- [#7207](https://github.com/multiversx/mx-chain-go/pull/7207) On Executed Block Clean
- [#7202](https://github.com/multiversx/mx-chain-go/pull/7202) On Proposed Block Validation
- [#7183](https://github.com/multiversx/mx-chain-go/pull/7183) Own Shard Tracker
- [#7180](https://github.com/multiversx/mx-chain-go/pull/7180) Verify Execution Results
- [#7198](https://github.com/multiversx/mx-chain-go/pull/7198) Fixed Vulnerabilities
- [#7179](https://github.com/multiversx/mx-chain-go/pull/7179) Refactor Hdrs For Block
- [#7200](https://github.com/multiversx/mx-chain-go/pull/7200) Refactor Hdrs For Block Tests
- [#7178](https://github.com/multiversx/mx-chain-go/pull/7178) Blocks Executor
- [#7137](https://github.com/multiversx/mx-chain-go/pull/7137) Gas Consumption
- [#7182](https://github.com/multiversx/mx-chain-go/pull/7182) Eviction Test Fix
- [#7192](https://github.com/multiversx/mx-chain-go/pull/7192) Todos Fixes
- [#7189](https://github.com/multiversx/mx-chain-go/pull/7189) Account Not Found 08 11
- [#7181](https://github.com/multiversx/mx-chain-go/pull/7181) Guarded Relayed
- [#7176](https://github.com/multiversx/mx-chain-go/pull/7176) Adapt Integration Tests Sub Second
- [#7177](https://github.com/multiversx/mx-chain-go/pull/7177) Logs Fixes
- [#7170](https://github.com/multiversx/mx-chain-go/pull/7170) Supernova Into Mempool 2025 08 06
- [#7162](https://github.com/multiversx/mx-chain-go/pull/7162) Supernova Integration Tests
- [#7175](https://github.com/multiversx/mx-chain-go/pull/7175) Mempool Session Account Not Found 08 07
- [#7173](https://github.com/multiversx/mx-chain-go/pull/7173) Request Handler Refactor
- [#7174](https://github.com/multiversx/mx-chain-go/pull/7174) Adapt Process Wait Time
- [#7161](https://github.com/multiversx/mx-chain-go/pull/7161) Increase Request Time
- [#7172](https://github.com/multiversx/mx-chain-go/pull/7172) Fix Sync Wait Time
- [#7171](https://github.com/multiversx/mx-chain-go/pull/7171) Update Unmarshallheader
- [#7144](https://github.com/multiversx/mx-chain-go/pull/7144) Economics Transition Rewards Tests
- [#7146](https://github.com/multiversx/mx-chain-go/pull/7146) Arithmetic Epoch Provider Transition
- [#7168](https://github.com/multiversx/mx-chain-go/pull/7168) Integrate New Header
- [#7111](https://github.com/multiversx/mx-chain-go/pull/7111) Economics Transition Inflation Rate
- [#7165](https://github.com/multiversx/mx-chain-go/pull/7165) System Smart Contracts Config Todos
- [#7159](https://github.com/multiversx/mx-chain-go/pull/7159) Feat Mempool Into Supernova June 16
- [#7153](https://github.com/multiversx/mx-chain-go/pull/7153) Get Transactions From Miniblocks
- [#7160](https://github.com/multiversx/mx-chain-go/pull/7160) Disable Broadcast Tx Statistics
- [#7148](https://github.com/multiversx/mx-chain-go/pull/7148) Handle Round Constants
- [#7155](https://github.com/multiversx/mx-chain-go/pull/7155) Fix Consensus State Locks
- [#7154](https://github.com/multiversx/mx-chain-go/pull/7154) Move Txcache Into Supernova
- [#7149](https://github.com/multiversx/mx-chain-go/pull/7149) Adapt Supernova Round Related Configs
- [#7081](https://github.com/multiversx/mx-chain-go/pull/7081) Pending Results Tracker
- [#7151](https://github.com/multiversx/mx-chain-go/pull/7151) Fix Round State Locks
- [#7147](https://github.com/multiversx/mx-chain-go/pull/7147) Txpool Profile
- [#7139](https://github.com/multiversx/mx-chain-go/pull/7139) Breadcrumbs Balance Validation
- [#7143](https://github.com/multiversx/mx-chain-go/pull/7143) Cleanup From Community
- [#7145](https://github.com/multiversx/mx-chain-go/pull/7145) Chronology Watchdog Transition
- [#7121](https://github.com/multiversx/mx-chain-go/pull/7121) Benchmarks Txpool
- [#7136](https://github.com/multiversx/mx-chain-go/pull/7136) Breadcrumbs Validation
- [#7122](https://github.com/multiversx/mx-chain-go/pull/7122) Header Logging
- [#7140](https://github.com/multiversx/mx-chain-go/pull/7140) Fix Genesis Check Before Activation
- [#7138](https://github.com/multiversx/mx-chain-go/pull/7138) Fix Round Index Check
- [#7125](https://github.com/multiversx/mx-chain-go/pull/7125) Virtual Record Fixes
- [#7133](https://github.com/multiversx/mx-chain-go/pull/7133) Optimize Compile Breadcrumb
- [#7135](https://github.com/multiversx/mx-chain-go/pull/7135) Mini Block Selection Session
- [#7127](https://github.com/multiversx/mx-chain-go/pull/7127) Missing Data Resolver
- [#7128](https://github.com/multiversx/mx-chain-go/pull/7128) Fix Genesis Time Check
- [#7120](https://github.com/multiversx/mx-chain-go/pull/7120) Reafactor Selection Tracker
- [#7113](https://github.com/multiversx/mx-chain-go/pull/7113) Txpool Integration Tests
- [#7105](https://github.com/multiversx/mx-chain-go/pull/7105) Fix Chain Parameters Metrics
- [#7104](https://github.com/multiversx/mx-chain-go/pull/7104) Fix Round Handler Transition
- [#7117](https://github.com/multiversx/mx-chain-go/pull/7117) Adjust Mempool Tests 07 16
- [#7097](https://github.com/multiversx/mx-chain-go/pull/7097) Ntp Syncer Round Changes
- [#7102](https://github.com/multiversx/mx-chain-go/pull/7102) Refactor Enable Rounds Handler
- [#7095](https://github.com/multiversx/mx-chain-go/pull/7095) Adapt Api Outport Ms Timestamp
- [#7107](https://github.com/multiversx/mx-chain-go/pull/7107) Virtual Session Provider
- [#7103](https://github.com/multiversx/mx-chain-go/pull/7103) Replace Session Wrapper
- [#7093](https://github.com/multiversx/mx-chain-go/pull/7093) Integration Tests Mempool
- [#7090](https://github.com/multiversx/mx-chain-go/pull/7090) Supernova Epoch Transition
- [#7091](https://github.com/multiversx/mx-chain-go/pull/7091) Virtual Session Implementation
- [#7086](https://github.com/multiversx/mx-chain-go/pull/7086) Virtual Records
- [#7085](https://github.com/multiversx/mx-chain-go/pull/7085) Derive Virtual Session
- [#7080](https://github.com/multiversx/mx-chain-go/pull/7080) Compiling Breadcrumbs
- [#7016](https://github.com/multiversx/mx-chain-go/pull/7016) Tx Broadcast Statistics
- [#7038](https://github.com/multiversx/mx-chain-go/pull/7038) Chain Simulator Round Milliseconds
- [#7047](https://github.com/multiversx/mx-chain-go/pull/7047) Unittests_txheap
- [#6995](https://github.com/multiversx/mx-chain-go/pull/6995) Fix Ntp Query
- [#7077](https://github.com/multiversx/mx-chain-go/pull/7077) Further Config Updates
- [#7076](https://github.com/multiversx/mx-chain-go/pull/7076) Add Supernova Activation Flag
- [#6994](https://github.com/multiversx/mx-chain-go/pull/6994) Round To Milliseconds
- [#7057](https://github.com/multiversx/mx-chain-go/pull/7057) Header Queue
- [#7065](https://github.com/multiversx/mx-chain-go/pull/7065) Virtual Selection Session
- [#7062](https://github.com/multiversx/mx-chain-go/pull/7062) Selection Tracker
- [#7051](https://github.com/multiversx/mx-chain-go/pull/7051) Barnard Into Mempool Supernova June 16
- [#7045](https://github.com/multiversx/mx-chain-go/pull/7045) MX 16970 Selection Options
- [#6993](https://github.com/multiversx/mx-chain-go/pull/6993) Constants Config
- [#7036](https://github.com/multiversx/mx-chain-go/pull/7036) Barnard Into Mempool Supernova
- [#7015](https://github.com/multiversx/mx-chain-go/pull/7015) Selection Gas To Config
- [#7020](https://github.com/multiversx/mx-chain-go/pull/7020) Unittests Txbyhashmap
- [#7004](https://github.com/multiversx/mx-chain-go/pull/7004) Upperbounds Config
- [#7014](https://github.com/multiversx/mx-chain-go/pull/7014) Selection Const To Config
- [#7018](https://github.com/multiversx/mx-chain-go/pull/7018) Select Txs Refactor
- [#7013](https://github.com/multiversx/mx-chain-go/pull/7013) Refactor Txcache
- [#7005](https://github.com/multiversx/mx-chain-go/pull/7005) Change Num Of Rounds Per Epoch
- [#7012](https://github.com/multiversx/mx-chain-go/pull/7012) Master Into Mempool Supernova
- [#6854](https://github.com/multiversx/mx-chain-go/pull/6854) MX 16624 Move Txcache
- [#7488](https://github.com/multiversx/mx-chain-go/pull/7488) Fix Metrics Chain Simulator
- [#7424](https://github.com/multiversx/mx-chain-go/pull/7424) Do Not Change Activation Round In Case Of Correct Values Supernova
- [#7331](https://github.com/multiversx/mx-chain-go/pull/7331) Termui Added Metrics Update
