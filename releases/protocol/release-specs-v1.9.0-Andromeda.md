[comment]: <> (tags/v1.9.0)

# Contents

This document explains the contents of the release codenamed Andromeda. 

Although it can be considered one big feature, this document is split into three features, containing detailed insights of the feature along with the external impact and the relevant pull requests list.

This documentation is relevant for the `tags/v1.9.0` tag release.

# Features

## 1. Consensus size changes [#4843](https://github.com/multiversx/mx-chain-go/pull/4843)

Nodes setup was only able to handle a static number of nodes in consensus (for shard and meta).

With this feature, the chain parameters(round duration, consensus group size, minimum number of nodes per shard, hysteresis and adaptivity) are now configurable, allowing dynamic changes at any given epoch.

### Impact:

* Configuration changes:
  * `config.toml` now holds a new structure, `ChainParametersByEpoch` that facilitates the configuration of these parameters for a given epoch
  * `nodesSetup.json` only holds the genesis start time and the initial nodes
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#4742](https://github.com/multiversx/mx-chain-go/pull/4742) - Consensus group size by epoch
- [#4797](https://github.com/multiversx/mx-chain-go/pull/4797) - Consensus group size changes part 2
- [#4799](https://github.com/multiversx/mx-chain-go/pull/4799) - Nodes shuffler refactoring
- [#4806](https://github.com/multiversx/mx-chain-go/pull/4806) - Update index hashed nodes coordinator
- [#4819](https://github.com/multiversx/mx-chain-go/pull/4819) - Fixes for nodes coordinator with chain parameters
- [#4927](https://github.com/multiversx/mx-chain-go/pull/4927) - Chain parameters notifier

## 2. Fixed ordering in consensus [#6436](https://github.com/multiversx/mx-chain-go/pull/6436)

The order of signature aggregation was fixed to the order of the eligible nodes decided at epoch start. As a result, each eligible node is now required to sign.

This simplifies the equivalent proof mechanics verification as it can be almost self-contained during an epoch.

### Impact:

* There is a new enable epoch definition for this feature, called `FixedOrderInConsensusEnableEpoch`
* `config.toml` holds a new entry for the newly added `ChainParametersByEpoch` with the full size consensus
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#6436](https://github.com/multiversx/mx-chain-go/pull/6436) - Fixed order consensus
- [#6448](https://github.com/multiversx/mx-chain-go/pull/6448) - Fixed ordering for consensus group after flag activation

## 3. Equivalent proofs [#5786](https://github.com/multiversx/mx-chain-go/pull/5786)

Equivalent proofs represent consensus proof data which includes aggregated signature, the corresponding public keys bitmap, and other fields like round and epoch used for validation.

Previously, only the leader handled the aggregated signature and set it on the initially proposed block, but now it can be created and propagated as a separate equivalent proof by every eligible node in consensus. These proofs are equivalent since they represent a majority from the eligible nodes in consensus.

An equivalent consensus proof will be propagated only once by every node, either if it was created by the node or if it was the first proof received and validated by the node.

The proof for the current block will be included in the next block structure. A particular block is considered final if there exists an equivalent proof for it.

The sync process has been updated to process a block only when there is a corresponding proof for it.

### Impact:

* There is a new enable epoch definition for this feature, called `FixedOrderInConsensusEnableEpoch`
* `config.toml` changes:
  * new configuration for the data pool holding equivalent proofs
  * new configuration for the intercepted data verifier
* Header structure was adapted to hold the proof of the previous block. The header proof is a new data structure that holds:
  * important header information such as bitmap, aggregated signature and header hash;
  * other information used for validation, such as header epoch, nonce, shard, round and start of epoch details.
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#5666](https://github.com/multiversx/mx-chain-go/pull/5666) - Equivalent messages filter on consensus topic
- [#5756](https://github.com/multiversx/mx-chain-go/pull/5756) - SubroundBlock changes
- [#5760](https://github.com/multiversx/mx-chain-go/pull/5760) - SubroundSignature changes
- [#5781](https://github.com/multiversx/mx-chain-go/pull/5781) - SubroundEndRound changes
- [#5790](https://github.com/multiversx/mx-chain-go/pull/5790) - Gradual broadcast mechanism
- [#5855](https://github.com/multiversx/mx-chain-go/pull/5855) - Fixes after tests
- [#6388](https://github.com/multiversx/mx-chain-go/pull/6388) - Increase coverage for subroundStartRound
- [#6392](https://github.com/multiversx/mx-chain-go/pull/6392) - Delayed broadcaster mock
- [#6401](https://github.com/multiversx/mx-chain-go/pull/6401) - Increase test coverage for metaChainMessenger and chronology
- [#6402](https://github.com/multiversx/mx-chain-go/pull/6402) - Multikey signatures on goroutines
- [#6405](https://github.com/multiversx/mx-chain-go/pull/6405) - Multikey signatures throttler
- [#6408](https://github.com/multiversx/mx-chain-go/pull/6408) - Refactor to use common functions for single/multi-key
- [#6411](https://github.com/multiversx/mx-chain-go/pull/6411) - Moving/removing consensus package mocks to testscommon
- [#6415](https://github.com/multiversx/mx-chain-go/pull/6415) - Increase coverage for nodesSetup
- [#6424](https://github.com/multiversx/mx-chain-go/pull/6424) - Benchmark signature verification
- [#6431](https://github.com/multiversx/mx-chain-go/pull/6431) - Adapt final info message check
- [#6437](https://github.com/multiversx/mx-chain-go/pull/6437) - Broadcast header on subroundBlock, on the shard_meta topic
- [#6456](https://github.com/multiversx/mx-chain-go/pull/6456) - Equivalent proofs pool
- [#6457](https://github.com/multiversx/mx-chain-go/pull/6457) - EquivalentProofsTopic + interceptor
- [#6462](https://github.com/multiversx/mx-chain-go/pull/6462) - Intercepted message validation optimization
- [#6479](https://github.com/multiversx/mx-chain-go/pull/6479) - Register consensus received header handler
- [#6483](https://github.com/multiversx/mx-chain-go/pull/6483) - Refactor consensus subrounds
- [#6501](https://github.com/multiversx/mx-chain-go/pull/6501) - Integrate proof check on sync
- [#6513](https://github.com/multiversx/mx-chain-go/pull/6513) - Metachain notarizez based on equivalent proofs
- [#6515](https://github.com/multiversx/mx-chain-go/pull/6515) - Fix integration tests - part 1
- [#6516](https://github.com/multiversx/mx-chain-go/pull/6516) - Final proof is now sent on the common topic with META
- [#6535](https://github.com/multiversx/mx-chain-go/pull/6535) - Refactor v2 subround block
- [#6537](https://github.com/multiversx/mx-chain-go/pull/6537) - Cleanup consensus v2 signature subround
- [#6540](https://github.com/multiversx/mx-chain-go/pull/6540) - Cleanup consensus v2 subround end round
- [#6601](https://github.com/multiversx/mx-chain-go/pull/6601) - Subround endround v2 tests
- [#6659](https://github.com/multiversx/mx-chain-go/pull/6659) - Fix header verification after equivalent proofs
- [#6665](https://github.com/multiversx/mx-chain-go/pull/6665) - Fixes header sig verification
- [#6683](https://github.com/multiversx/mx-chain-go/pull/6683) - Subround endround v2 fixes
- [#6685](https://github.com/multiversx/mx-chain-go/pull/6685) - Adapt integration tests for consensus v2
- [#6686](https://github.com/multiversx/mx-chain-go/pull/6686) - Stabilization equivalent proofs
- [#6697](https://github.com/multiversx/mx-chain-go/pull/6697) - Check proof on activation block sync
- [#6698](https://github.com/multiversx/mx-chain-go/pull/6698) - Added checks on proofs parameters like epoch, nonce and shard
- [#6712](https://github.com/multiversx/mx-chain-go/pull/6712) - Change AddProof
- [#6717](https://github.com/multiversx/mx-chain-go/pull/6717) - Equivalent proofs stabilization 2
- [#6718](https://github.com/multiversx/mx-chain-go/pull/6718) - Fix missing RUnlock
- [#6724](https://github.com/multiversx/mx-chain-go/pull/6724) - Fix transition block
- [#6725](https://github.com/multiversx/mx-chain-go/pull/6725) - Proofs pool cleanup delta
- [#6727](https://github.com/multiversx/mx-chain-go/pull/6727) - Complete shardData.Epoch
- [#6728](https://github.com/multiversx/mx-chain-go/pull/6728) - Fix epoch check on shard data only after activation
- [#6729](https://github.com/multiversx/mx-chain-go/pull/6729) - Improved VerifyProofAgainstHeader with extra check on IsStartOfEpoch field
- [#6730](https://github.com/multiversx/mx-chain-go/pull/6730) - Early exist for shard data that should not have prev proof
- [#6732](https://github.com/multiversx/mx-chain-go/pull/6732) - Fix requesting attesting blocks after equivalent proofs
- [#6733](https://github.com/multiversx/mx-chain-go/pull/6733) - More fixes on the notarization part
- [#6735](https://github.com/multiversx/mx-chain-go/pull/6735) - Cosmetic changes
- [#6739](https://github.com/multiversx/mx-chain-go/pull/6739) - Fix shard stuck
- [#6742](https://github.com/multiversx/mx-chain-go/pull/6742) - Fixed sync issues
- [#6745](https://github.com/multiversx/mx-chain-go/pull/6745) - Fix shard final block at transition
- [#6749](https://github.com/multiversx/mx-chain-go/pull/6749) - Fix config for shard nodes
- [#6755](https://github.com/multiversx/mx-chain-go/pull/6755) - Fix sync on meta
- [#6759](https://github.com/multiversx/mx-chain-go/pull/6759) - Proper fix for meta sync
- [#6760](https://github.com/multiversx/mx-chain-go/pull/6760) - Fix consensus check
- [#6762](https://github.com/multiversx/mx-chain-go/pull/6762) - Cleanup delayed broadcast of proofs
- [#6764](https://github.com/multiversx/mx-chain-go/pull/6764) - Fixes for multikey mode, send proof from random managed key
- [#6769](https://github.com/multiversx/mx-chain-go/pull/6769) - Add header verification unit tests
- [#6771](https://github.com/multiversx/mx-chain-go/pull/6771) - Fix wrong size bitmap error on transition block
- [#6778](https://github.com/multiversx/mx-chain-go/pull/6778) - Reverted a previous commit leading to improper epoch usage for header verification
- [#6783](https://github.com/multiversx/mx-chain-go/pull/6783) - Set fork detector checkpoints on bootstrapper applyBlock
- [#6785](https://github.com/multiversx/mx-chain-go/pull/6785) - Fix some long tests
- [#6786](https://github.com/multiversx/mx-chain-go/pull/6786) - Better handling of missing attesting headers
- [#6787](https://github.com/multiversx/mx-chain-go/pull/6787) - Consensus transition speedup
- [#6788](https://github.com/multiversx/mx-chain-go/pull/6788) - Trigger activation fixes
- [#6794](https://github.com/multiversx/mx-chain-go/pull/6794) - Fix writing on channel when a block is requested in order to attest the current one
- [#6804](https://github.com/multiversx/mx-chain-go/pull/6804) - Wait for epoch start block if not available
- [#6805](https://github.com/multiversx/mx-chain-go/pull/6805) - Block body mutex
- [#6806](https://github.com/multiversx/mx-chain-go/pull/6806) - Avoid executing twice the block
- [#6807](https://github.com/multiversx/mx-chain-go/pull/6807) - Skip signatures check for observers
- [#6808](https://github.com/multiversx/mx-chain-go/pull/6808) - Fixes delayed broadcasting
- [#6810](https://github.com/multiversx/mx-chain-go/pull/6810) - Extra log on sync block
- [#6813](https://github.com/multiversx/mx-chain-go/pull/6813) - Fix for "accumulated fees do not match" due to concurrent commit and scheduled processing
- [#6814](https://github.com/multiversx/mx-chain-go/pull/6814) - Previous header proof in block structure
- [#6816](https://github.com/multiversx/mx-chain-go/pull/6816) - Remove old consensus test with equivalent proofs
- [#6819](https://github.com/multiversx/mx-chain-go/pull/6819) - Remove close err chan
- [#6822](https://github.com/multiversx/mx-chain-go/pull/6822) - Fix set of final block info after activation
- [#6823](https://github.com/multiversx/mx-chain-go/pull/6823) - When no block received, try removing the first requested block
- [#6825](https://github.com/multiversx/mx-chain-go/pull/6825) - Add proof to pool on transition
- [#6828](https://github.com/multiversx/mx-chain-go/pull/6828) - Fix error on observers before activation
- [#6830](https://github.com/multiversx/mx-chain-go/pull/6830) - Equivalent proofs unit tests
- [#6831](https://github.com/multiversx/mx-chain-go/pull/6831) - Add process block stopwatch
- [#6835](https://github.com/multiversx/mx-chain-go/pull/6835) - Check shard data only on meta nodes in interceptor
- [#6836](https://github.com/multiversx/mx-chain-go/pull/6836) - Subround endround equivalent proofs unit tests
- [#6837](https://github.com/multiversx/mx-chain-go/pull/6837) - Add state statistics for search first
- [#6842](https://github.com/multiversx/mx-chain-go/pull/6842) - Fix Andromeda import db
- [#6847](https://github.com/multiversx/mx-chain-go/pull/6847) - Add storage statistics for persister write operation
- [#6849](https://github.com/multiversx/mx-chain-go/pull/6849) - Chain simulator integration andromeda
- [#6851](https://github.com/multiversx/mx-chain-go/pull/6851) - Get bitmap and signature from proof for api block
- [#6850](https://github.com/multiversx/mx-chain-go/pull/6850) - Andromeda increase coverage part 1
- [#6853](https://github.com/multiversx/mx-chain-go/pull/6853) - Andromeda increase coverage part 2
- [#6860](https://github.com/multiversx/mx-chain-go/pull/6860) - Andromeda increase coverage part 3
- [#6869](https://github.com/multiversx/mx-chain-go/pull/6869) - Andromeda increase coverage part 4
- [#6857](https://github.com/multiversx/mx-chain-go/pull/6857) - Equivalent messages anti flood
- [#6862](https://github.com/multiversx/mx-chain-go/pull/6862) - Skip signer indices indexing
- [#6863](https://github.com/multiversx/mx-chain-go/pull/6863) - Fixed area not protected by mutex + tests
- [#6867](https://github.com/multiversx/mx-chain-go/pull/6867) - Outport finalized state updates
- [#6873](https://github.com/multiversx/mx-chain-go/pull/6873) - Invalid signers cache
- [#6875](https://github.com/multiversx/mx-chain-go/pull/6875) - Integration test rewards Andromeda
- [#6877](https://github.com/multiversx/mx-chain-go/pull/6877) - Ratings config per epoch
- [#6878](https://github.com/multiversx/mx-chain-go/pull/6878) - Proofs pool improvements
- [#6880](https://github.com/multiversx/mx-chain-go/pull/6880) - General review fixes
- [#6882](https://github.com/multiversx/mx-chain-go/pull/6882) - Recursive call to doEndRoundJobByNode in order to wait more signatures
- [#6884](https://github.com/multiversx/mx-chain-go/pull/6884) - Optimize startup for internal testnets
- [#6885](https://github.com/multiversx/mx-chain-go/pull/6885) - Fix metrics for leader todo
- [#6886](https://github.com/multiversx/mx-chain-go/pull/6886) - Remove Todo's Andromeda
- [#6887](https://github.com/multiversx/mx-chain-go/pull/6887) - Proofs pool upsert
- [#6889](https://github.com/multiversx/mx-chain-go/pull/6889) - Minor config compression
- [#6892](https://github.com/multiversx/mx-chain-go/pull/6892) - More tests for sendProof
- [#6895](https://github.com/multiversx/mx-chain-go/pull/6895) - Remove signed blocks threshold
- [#6896](https://github.com/multiversx/mx-chain-go/pull/6896) - Fix sync tests
- [#6898](https://github.com/multiversx/mx-chain-go/pull/6898) - Fix keys management max rounds of inactivity
- [#6899](https://github.com/multiversx/mx-chain-go/pull/6899) - Integration tests rewards with txs
- [#6901](https://github.com/multiversx/mx-chain-go/pull/6901) - Sed chain parameters in respect with local testnet scripts
