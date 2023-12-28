[comment]: <> (last PR processed #5580)

# Contents

This document explains the contents of the rc/v1.6.0 release codenamed Sirius. It is split in 3 sections: 
- the **features** list containing detailed insights of the feature along with the external impact and the relevant 
pull requests list
- the **smaller features and fixes** area contains the one-pull request small features or fixes along with the 
external impact details
- the **merges** area contains the list of pull requests used to update feature branches as the work progressed on 
multiple areas at once. It is here for reference purposes only.

This documentation is relevant for the `tags/v1.6.7` tag release.

# Features

## 1. Optimise consensus signature check [#4467](https://github.com/multiversx/mx-chain-go/pull/4467)

This feature brings several changes on the way block signatures are handled during consensus in order to optimize the CPU usage.
Each signature share used in the aggregation of signatures for the block consensus proof, was previously individually verified.
Assuming all validators are honest, the verification takes ~3ms per signature. In metachain, each block accumulates ~400 signatures
which are individually checked by all participants in consensus, taking a cumulative ~1.2s CPU time for each block that passes consensus.
This 1.2 seconds required for the metachain and ~ 200 ms for the shards each round (for the synchronized nodes) can be optimized. 
This can be achieved by assuming all signature shares are valid during aggregation, and then performing verification on the aggregated result.
The cost of the verification of the aggregated signature was optimized by the previous PR #4314 down to ~3ms as well.
This however can only be saved if all aggregated signatures are valid, otherwise, if the aggregated signature does not verify we will have an extra cost of 3ms,
as the leader will aggregate first the signatures and then verify the aggregated signature. If the aggregated signature does not verify, the leader will
have to check the signatures individually. In order to avoid the increase and instead always have this optimization, a penalty for providing invalid
signatures will be implemented which we call a pseudo slashing, that should provide enough disincentive. The pseudo slashing is implemented through the addition
of an extra message with the proof for received invalid signatures, sent on the consensus. This message is not mandatory to be sent during a consensus round,
but if it is sent, the proof will be verified and used by nodes listening to the consensus messages to temporarily blacklist the misbehaving nodes.
While blacklisted, the messages from the nodes in the blacklist will be dropped and this will cause rating drop and the eventual jailing, that penalize the node
through the unjailing fee, missing block rewards/fees and queue time for becoming eligible for consensus again.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4123](https://github.com/multiversx/mx-chain-go/pull/4123) - Optimistic signature aggregation on consensus
- [#4460](https://github.com/multiversx/mx-chain-go/pull/4460) - Create a separate component in p2p package
- [#4466](https://github.com/multiversx/mx-chain-go/pull/4466) - Added direct send message signing to ensure behavior consistency in the p2p network messenger
- [#4506](https://github.com/multiversx/mx-chain-go/pull/4506) - Added integration test for consensus signing
- [#4522](https://github.com/multiversx/mx-chain-go/pull/4522) - Handle metachain in consensus integration tests
- [#4269](https://github.com/multiversx/mx-chain-go/pull/4269) - Pseudo slashing for invalid signers
- [#4307](https://github.com/multiversx/mx-chain-go/pull/4307) - Added the possibility to blacklist peers that create invalid signatures on the consensus topic
- [#4630](https://github.com/multiversx/mx-chain-go/pull/4630) - Handle invalid signers similar as in the verification case
- [#4647](https://github.com/multiversx/mx-chain-go/pull/4647) - Create p2p signer component in factory
- [#4682](https://github.com/multiversx/mx-chain-go/pull/4682) - Proper release for p2p repo
- [#4710](https://github.com/multiversx/mx-chain-go/pull/4710) - Fix pubkey in log message
- [#4749](https://github.com/multiversx/mx-chain-go/pull/4749) - Call the Close() function in components
- [#4764](https://github.com/multiversx/mx-chain-go/pull/4764) - Fixes after review on feat branch: fix indentation, use timer in blacklist component
- [#4768](https://github.com/multiversx/mx-chain-go/pull/4768) - Added extra delay for the timeout values in consensus integration tests

## 2. Refactor resolvers [#4692](https://github.com/multiversx/mx-chain-go/pull/4692)

This feature solves a technical debt by splitting the functionality of a resolver component in 2.
The resolver component should be able only to respond to requests from the p2p network and the requester should 
be able only to request missing information (send the request packet on the p2p network)

### Impact:
* No activation epoch
* There is a small renaming in the `config.toml` file, the section `[Resolvers]` was changed to `[Requesters]`
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4683](https://github.com/multiversx/mx-chain-go/pull/4683) - Created requests handler for each resolver
- [#4700](https://github.com/multiversx/mx-chain-go/pull/4700) - Added containers, factories and topic sender
- [#4705](https://github.com/multiversx/mx-chain-go/pull/4705) - Added unit tests
- [#4709](https://github.com/multiversx/mx-chain-go/pull/4709) - Renamed and moved components
- [#4715](https://github.com/multiversx/mx-chain-go/pull/4715) - Removed the requester logic out from resolvers along with multiple stubs

## 3. Multikey [#4741](https://github.com/multiversx/mx-chain-go/pull/4741)

This feature added multikey support that will enable the node to sign on behalf of more than one key.
For it to function, a multikey node will be required for each
shard of the chain (including the metachain) and the exact set of keys should be given to all the nodes.
A detailed description of the feature is available [on the multikey docs page](https://docs.multiversx.com/validators/key-management/multikey-nodes)

### Impact:
* No activation epoch
* In terms of the configuration files, we have this change list:
    * in `config.toml` file there is a new config value `PeerAuthenticationTimeBetweenChecksInSec` set to 6 seconds
    * in `prefs.toml` file there is a new slice section called `[[NamedIdentity]]` to manually specify the named identity of a set of BLS keys
    * the node will require a file called allValidatorsKeys.pem containing all handled keys
* No binary flags changes
* There are four new API endpoints to provide managed keys information: `/node/managed-keys`, `/node/managed-keys/count`, `/node/managed-keys/eligible` and `/node/managed-keys/waiting`

### Relevant PRs:
- [#4253](https://github.com/multiversx/mx-chain-go/pull/4253) - Added support for the multikey feature in network messenger
- [#4261](https://github.com/multiversx/mx-chain-go/pull/4261) - Added keysHolder struct, further refactoring in p2p package: extracted a new package + new struct
- [#4266](https://github.com/multiversx/mx-chain-go/pull/4266) - Added redundancy support for keysHolder component
- [#4381](https://github.com/multiversx/mx-chain-go/pull/4381) - Integrated changes on prefs config, which now provides identities that runs nodes in multikey mode
- [#4394](https://github.com/multiversx/mx-chain-go/pull/4394) - Refactored the peer authentication sender so it will be compatible with the multikey system
- [#4414](https://github.com/multiversx/mx-chain-go/pull/4414) - Implemented 2 types of peer authentication available, one for multikey and the normal operation one
- [#4416](https://github.com/multiversx/mx-chain-go/pull/4416) - Made the heartbeat sender to be compatible with the multikey system
- [#4455](https://github.com/multiversx/mx-chain-go/pull/4455) - Added a factory for the 2 heartbeat sender implementations
- [#4463](https://github.com/multiversx/mx-chain-go/pull/4463) - Integrated the multikey components in consensus
- [#4484](https://github.com/multiversx/mx-chain-go/pull/4484) - Renamed keysHolder to managedPeersHolder
- [#4747](https://github.com/multiversx/mx-chain-go/pull/4747) - Refactored nodes keys in integration tests
- [#4748](https://github.com/multiversx/mx-chain-go/pull/4748) - Implemented keysHandler component
- [#4620](https://github.com/multiversx/mx-chain-go/pull/4620) - Added integration test to prove the multikey feature works as expected
- [#4750](https://github.com/multiversx/mx-chain-go/pull/4750) - Changes in crypto factory to take into account the allValidatorsKeys.pem file
- [#4757](https://github.com/multiversx/mx-chain-go/pull/4757) - Updated testnet scripts to support multikey
- [#4758](https://github.com/multiversx/mx-chain-go/pull/4758) - Refactored the private keys usage in consensus
- [#4947](https://github.com/multiversx/mx-chain-go/pull/4947) - Added IsMultiKeyMode method to ManagedPeersHolder interface, added more unit tests
- [#5057](https://github.com/multiversx/mx-chain-go/pull/5057) - Proper releases for used libraries
- [#5574](https://github.com/multiversx/mx-chain-go/pull/5574) - Fixed the heartbeat messages for the identities handled by the multikey node
- [#5597](https://github.com/multiversx/mx-chain-go/pull/5597) - Fixed managed peers holder to avoid panics in case of a badly configured node
- [#5601](https://github.com/multiversx/mx-chain-go/pull/5601) - Fixed issues between the multikey feature and the redundancy sub-system
- [#5782](https://github.com/multiversx/mx-chain-go/pull/5782) - Fixed managed peers holder

There is a related feature called feat/multikey metrics [#5362](https://github.com/multiversx/mx-chain-go/pull/5362) that added new API endpoint routes to provide information regarding
the managed keys status

### Relevant PRs:
- [#5356](https://github.com/multiversx/mx-chain-go/pull/5356) - Created a new component needed to provide managed keys info called managedPeersMonitor
- [#5357](https://github.com/multiversx/mx-chain-go/pull/5357) - Added new API endpoints that will provide valuable information while the node is running in multikey mode
- [#5354](https://github.com/multiversx/mx-chain-go/pull/5354) - Made the metrics update whenever one of the managed keys is leader
- [#5371](https://github.com/multiversx/mx-chain-go/pull/5371) - Fixed a member initialization that caused panic
- [#5375](https://github.com/multiversx/mx-chain-go/pull/5375) - Added `/node/managed-keys` endpoint

## 4. PubKeyConverter refactor [#4716](https://github.com/multiversx/mx-chain-go/pull/4716)

The original implementation of the `PubkeyConverter.Encode` method did not propagate the error to its caller.
Instead, it logged the error and returned an empty string. This behavior can be problematic as it might hide potential issues and security threats.
Furthermore, the logger's presence in the PubKeyConverter constructor became unnecessary, therefore the `hrp` was given as input, especially considering
our vision for sovereign shards where each shard could have its unique human-readable part (`hrp`).

### Impact:
* No activation epoch
* There is a configuration option the `config.toml` file, the section `[AddressPubkeyConverter]` now contains a parameter called `Hrp`
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4708](https://github.com/multiversx/mx-chain-go/pull/4708) - Facilitate the change of bech32 addresses prefix format for other chains, refactored the Encode and EncodeSlice functions, added SilentEncode function
- [#5098](https://github.com/multiversx/mx-chain-go/pull/5098) - Fixes after review
- [#5109](https://github.com/multiversx/mx-chain-go/pull/5109) - Proper releases

## 5. Peers rating handler fixes [#4800](https://github.com/multiversx/mx-chain-go/pull/4800)

This feature solves several bugs in the p2p peers rating components that are used whenever a node will try to determine which p2p peers have a better response history to
maximize the chance of receiving a response.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* There is one new API endpoint called `/node/connected-peers-ratings` that will output the ratings of the connected p2p peers.
These ratings are computed by the running node and can differ from other instances connected to the same peers, so it is a strictly local computed value for each connected peer

### Relevant PRs:
- [#4709](https://github.com/multiversx/mx-chain-go/pull/4709) - Components renaming, moving and integration
- [#4755](https://github.com/multiversx/mx-chain-go/pull/4755) - Added DecreaseRating call
- [#4780](https://github.com/multiversx/mx-chain-go/pull/4780) - Added call to decrease p2p rating and added integration test
- [#4781](https://github.com/multiversx/mx-chain-go/pull/4781) - Added integration test and updated mx-chain-p2p-go library
- [#5050](https://github.com/multiversx/mx-chain-go/pull/5050) - Libraries update, tests fixes & cleanup
- [#5099](https://github.com/multiversx/mx-chain-go/pull/5099) - Integrated the new mx-chain-p2p-go to have the latest peers ratings handler
- [#5587](https://github.com/multiversx/mx-chain-go/pull/5587) - Added new p2p metrics, fixed a bad logging in the p2p components

## 6. Governance v3 [#4879](https://github.com/multiversx/mx-chain-go/pull/4879)

The voting smart contract is implemented in go, and it is running on the system VM alongside the other known 
contract (staking, auction, delegation). The voting contract currently does not have any enforcement, 
proposals are voted for to keep track of decisions.

The contract has a config which is set at genesis time:
```protobuf
message GovernanceConfig {
  int32 MinQuorum = 1 [(gogoproto.jsontag) = "MinQuorum"];
  int32 MinPassThreshold = 2 [(gogoproto.jsontag) = "MinPassThreshold"];
  int32 MinVetoThreshold = 3 [(gogoproto.jsontag) = "MinVetoThreshold"];
  int32 MaxDuration = 4 [(gogoproto.jsontag) = "MaxDuration"];
  bytes ProposalFee = 5 [(gogoproto.jsontag) = "ProposalFee", 
}
```

`MinQuorum`, `MinPassThreshold`, `MinVetoThreshold` can be expressed in terms of either stake or voting power, 
the result is the same.
We also have a `MaxDuration` - which is a maximum number of epochs a proposal can run. This prevents users 
from locking their tokens in the Governance contract for a very long time if the proposal is made 
incorrectly or malicious with a very high duration;
a `ProposalFee` - to prevent spam (when more community addresses are whitelisted or the governance becomes 
completely open);

General change proposal

```protobuf
message GeneralProposal {
  bytes IssuerAddress = 1 [(gogoproto.jsontag) = "IssuerAddress"];
  bytes GitHubCommit = 2 [(gogoproto.jsontag) = "GitHubCommit"];
  uint64 StartVoteNonce = 3 [(gogoproto.jsontag) = "StartVoteNonce"];
  uint64 EndVoteNonce = 4 [(gogoproto.jsontag) = "EndVoteNonce"];
  int32 Yes = 5 [(gogoproto.jsontag) = "Yes"];
  int32 No = 6 [(gogoproto.jsontag) = "No"];
  int32 Veto = 7 [(gogoproto.jsontag) = "Veto"];
  int32 Abstain = 8 [(gogoproto.jsontag) = "Abstain"];
  bool Voted = 9 [(gogoproto.jsontag) = "Voted"];
  repeated bytes Voters = 10 [(gogoproto.jsontag) = "Voters"];
  bytes TopReference = 11 [(gogoproto.jsontag) = "TopReference"];
}

```

Once a proposal gets registered and the proposal fee is paid - the voting can start from the start vote nonce 
until the end vote nonce. After the `EndVotePeriod` any whitelisted address can send one more transaction in 
order to close the proposal - at that time it will be calculated if the proposal is voted or not. With that 
transaction all the votes for that selected proposal will be deleted from the trie.
For every valid vote there will be a new entrance created in the trie with the key: `proposal+callerAddress`. 
These will be deleted when a proposal is finished. `ProposalFinish` call will clean all the storage and only 
after will set the proposal to the computed state - passed or not.
Same proposal ("GitHub commit") cannot be set more than once.

To deploy a proposal, one might use the transaction data field formatted as: 
```
proposal@<githubcommithash(40bytes as hex)>@startVoteEpoch@endVoteEpoch
```
ProposalCost: 1000eGLD

User makes a transaction with `vote@<proposalX>@<VoteType>`:
The governance contract will ask the staking, validator and delegation contracts how much stake/delegated 
eGLD he has. From the staked/delegated eGLD we compute the voting power using the linear formula.

The governance contract will compute the gas according to the number of storage GETS he needs to do to 
compute the voting power for each user. The user does not need to provide how much eGLD he staked/delegated, 
the governance contract will resolve this.

For contracts like MultiversX Community Delegation, Liquid Staking contracts, we made a new endpoint called 
`delegateVote@<proposalX>@<VoteType>@<delegateTo>@<balanceToVote>`

First we compute the total staked/delegated for that contract, compute if the 
`totalVoted += balanceToVote < totalStaked` for the contract. We assign the votes to the user the contract have 
delegated, recomputing the user’s voting power with the quadratic formula.

### Impact:
* No new activation epoch, will reuse the original defined one called `GovernanceEnableEpoch`
* There are changes to the configuration options in the `systemSmartContractsConfig.toml` file, `[GovernanceSystemSCConfig]` section
Also, the gas schedule files contain a new gas definition called `GetActiveFund`
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4910](https://github.com/multiversx/mx-chain-go/pull/4910) - Added new view functions for governance
- [#4913](https://github.com/multiversx/mx-chain-go/pull/4913) - Linter fixes, enabled feature in test configs
- [#4879](https://github.com/multiversx/mx-chain-go/pull/4879) - Main governance v3 branch
- [#5223](https://github.com/multiversx/mx-chain-go/pull/5223) - Added a fee when proposal is lost
- [#5750](https://github.com/multiversx/mx-chain-go/pull/5750) - Governance fixes

## 7. DNS v2 [#5045](https://github.com/multiversx/mx-chain-go/pull/5045)

Integrated new DNS functionalities:
* `saveUserName` can be called multiple times, and it will change the current username of 
the user
* `deleteUserName` will delete the username of the user
Both endpoints can be called only by special/whitelisted smart contracts

### Impact:
* There is a new enable epoch definition for this feature, called `KeepExecOrderOnCreatedSCRsEnableEpoch`
* There are changes to the configuration options in the `systemSmartContractsConfig.toml` file, `[GovernanceSystemSCConfig]` section
Also, the gas schedule files contain a new gas definition called `GetActiveFund`
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#5045](https://github.com/multiversx/mx-chain-go/pull/5045) - Main DNS v2 branch
- [#5046](https://github.com/multiversx/mx-chain-go/pull/5046) - Added semi-integration test for relayed dns scenario
- [#5073](https://github.com/multiversx/mx-chain-go/pull/5073) - Added the possibility to deploy and execute the DNS v2 contract

## 8. WebSocket outport driver [#5142](https://github.com/multiversx/mx-chain-go/pull/5142)

Refactored the WebSocket driver. The new drivers now support sending messages marshaled with json marshaller or the gogo proto marshalled.
The new WebSocketHost implementation can run in server or client mode.
A detailed description of the feature is available [on the indexer docs page](https://docs.multiversx.com/sdk-and-tools/indexer)

### Impact:
* No activation epoch
* There are changes in the `external.toml` file as there is a new optional section `[HostDriverConfig]` to be used with a generic web socket driver
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#5097](https://github.com/multiversx/mx-chain-go/pull/5097) - Refactored Driver and OutportHandler to work with proto messages instead of interfaces, added customizable marshaller on each Driver instance
- [#5129](https://github.com/multiversx/mx-chain-go/pull/5129) - Customizable marshaller for each Driver can now be loaded from the config file
- [#5135](https://github.com/multiversx/mx-chain-go/pull/5135) - Fixed the part where the transactions were fetched from the pool in the execution order component
- [#5170](https://github.com/multiversx/mx-chain-go/pull/5170) - Moved the WebSocketDriver (now named `HostDriver) in this repository. Integrated the FullDuplexHost from mx-chain-communication-go repository
- [#5252](https://github.com/multiversx/mx-chain-go/pull/5252) - Updated the version of the mx-chain-es-indexer-go library
- [#5265](https://github.com/multiversx/mx-chain-go/pull/5265) - Fixed the indexer creation
- [#5278](https://github.com/multiversx/mx-chain-go/pull/5278) - Added new configuration file definitions
- [#5286](https://github.com/multiversx/mx-chain-go/pull/5286) - Added a subscribe-call mechanism for the outport drivers to be able to request and get the current node settings
- [#5384](https://github.com/multiversx/mx-chain-go/pull/5384) - Added the possibility to define a set of WebSocket host drivers instead of just one
- [#5547](https://github.com/multiversx/mx-chain-go/pull/5547) - Added & populated new field in `AccountAdditionalData` structure 
- [#5580](https://github.com/multiversx/mx-chain-go/pull/5580) - Integrated new indexer version
- [#5594](https://github.com/multiversx/mx-chain-go/pull/5594) - Added versioning for WebSocket message

## 9. Sync missing trie nodes [#4616](https://github.com/multiversx/mx-chain-go/pull/4616)
If there was a bug during processing and a trie node was missing (either because it was deleted or because it was never saved in the first place),
that validator would have not been able to move forward with block processing.
When a missing trie node is reached, sync it from the network. In this way, the network nodes will be able to repair themselves.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4588](https://github.com/multiversx/mx-chain-go/pull/4588) - Propagate the getNodeFromDB error until baseSync. This will automatically trigger the state sync
- [#4724](https://github.com/multiversx/mx-chain-go/pull/4724) - Handle get nodes from db error in order to pass also the missing node hash
- [#4830](https://github.com/multiversx/mx-chain-go/pull/4830) - Removed constants for peer and user accounts key and use constants from dataRetriever storage units
- [#5188](https://github.com/multiversx/mx-chain-go/pull/5188) - Pass storageMarker to SyncAccounts() func. This way, a disabled storage marker can be given in case the db should not be marked as synced
- [#5217](https://github.com/multiversx/mx-chain-go/pull/5217) - Added a notifier in blockchainHook which notifies the state syncer that a data trie node is missing. This way the state syncer can start to sync the missing data trie nodes
- [#5258](https://github.com/multiversx/mx-chain-go/pull/5258) - Get identifier from trieStorageManager and not from storer
- [#5271](https://github.com/multiversx/mx-chain-go/pull/5271) - Fixed failing unit test
- [#5273](https://github.com/multiversx/mx-chain-go/pull/5273) - Proper releases for libraries

## 10. Balance data tries [#4636](https://github.com/multiversx/mx-chain-go/pull/4636)

The keys where the values are saved in a data trie are not random. Because of this, the data tries are not balanced, thus resulting in more intermediary nodes.
When saving some data in the data trie, do not save at `key` but rather at `hash(key)`. This way, the keys will be random, resulting in balanced data tries.
In order for this change to be backwards compatible, add Version field to trie nodes.
Nodes that are accessed will be automatically migrated to the new version (where the storage key is hash(key))
Added a builtin function that will migrate data trie nodes when it is called.
Added a new API endpoint which will return true if the data trie of a specified account is fully migrated.

### Impact:
* There is a new enable epoch definition for this feature, called `AutoBalanceDataTriesEnableEpoch`
* The gas schedule files are changed, there are 2 new gas costs added `TrieLoadPerNode` and `TrieStorePerNode`
* No binary flags changes
* There is one new API endpoint called `/address/:address/is-data-trie-migrated` that will return true if the account's data trie was migrated or false otherwise

### Relevant PRs:
- [#4640](https://github.com/multiversx/mx-chain-go/pull/4640) - Added marshaller to trackable data trie
- [#4633](https://github.com/multiversx/mx-chain-go/pull/4633) - Added a hasher on the NewTrackableDataTrie constructor
- [#4680](https://github.com/multiversx/mx-chain-go/pull/4680) - Propagate enableEpochsHandler to userAccount and other components
- [#4686](https://github.com/multiversx/mx-chain-go/pull/4686) - Pass a disabledTrieLeafParser() to GetAllLeavesOnChannel(). This will be changed to a new implementation of trieLeafParser in a future PR.
- [#4695](https://github.com/multiversx/mx-chain-go/pull/4695) - When inserting in a data trie, insert at hash(key). When getting from a data trie, first try to retrieve the val from hash(key), and if that does not work, try and get from key.
- [#4729](https://github.com/multiversx/mx-chain-go/pull/4729) - Revert some changes from previous PR
- [#4821](https://github.com/multiversx/mx-chain-go/pull/4821) - After unmarshal, also check that the trieData is not empty.
- [#4735](https://github.com/multiversx/mx-chain-go/pull/4735) - Added a new field to each type of trie node. A leaf node will remember it's own version, while branch nodes and extension nodes will save the version of their children
- [#4809](https://github.com/multiversx/mx-chain-go/pull/4809) - Added unit tests for trie nodes versioning
- [#4962](https://github.com/multiversx/mx-chain-go/pull/4962) - Refactored trackableDataTrie so that it can handle the trie nodes migration process in a generalized manner
- [#5056](https://github.com/multiversx/mx-chain-go/pull/5056) - Added the possibility to collect stats about trie nodes version during state snapshot.
- [#5115](https://github.com/multiversx/mx-chain-go/pull/5115) - Added tests for migrateDataTrie built-in function
- [#5127](https://github.com/multiversx/mx-chain-go/pull/5127) - Added an API endpoint which returns the status of the data trie migration
- [#5180](https://github.com/multiversx/mx-chain-go/pull/5180) - Updated libraries
- [#5194](https://github.com/multiversx/mx-chain-go/pull/5194) - Fixed a case in which Delete() is called twice in a row for the same key, code cleanup
- [#5197](https://github.com/multiversx/mx-chain-go/pull/5197) - Remove duplicated code: get from trie on RetrieveValue() and getOldValue()
- [#5242](https://github.com/multiversx/mx-chain-go/pull/5242) - Small stats refactor
- [#5282](https://github.com/multiversx/mx-chain-go/pull/5282) - Changed the epoch in which autoBalanceDataTries is enabled
- [#5298](https://github.com/multiversx/mx-chain-go/pull/5298) - Proper releases for libraries
- [#5611](https://github.com/multiversx/mx-chain-go/pull/5611) - Fixed an edge-case related to trie pruning + migrate data trie + revert  
- [#5579](https://github.com/multiversx/mx-chain-go/pull/5579) - Changed gas cost for migrate data trie built-in function

## 11. Sharded persister [#5010](https://github.com/multiversx/mx-chain-go/pull/5010)

Added the possibility to split a storer among more than one directory, each having its own level DB instance. The keys are routed in their respective data shard in the same
manner as wallet addresses are split among chain shards. The number of data shards is configurable and backward compatible as the information is saved in a file, in the same directory
where the data shards reside. This split will improve data access, especially when dealing with state trie nodes.

### Impact:
* No activation epoch
* There are changes in the `config.toml` file as some DB configurations now use the newly added options `ShardIDProviderType` and `NumShards`
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4979](https://github.com/multiversx/mx-chain-go/pull/4979) - Added config options for shard id persister, updated storage factory to include sharded persister
- [#5002](https://github.com/multiversx/mx-chain-go/pull/5002) - Set sharded DB setup per each storer separately
- [#5210](https://github.com/multiversx/mx-chain-go/pull/5210) - Cleanup unused code
- [#5543](https://github.com/multiversx/mx-chain-go/pull/5543) - Activated sharded persister for the UserAccounts and PeerAccounts tries
- [#5751](https://github.com/multiversx/mx-chain-go/pull/5751) - Use sharded persister for static storer

## 12. Trie sync optimizations [#5291](https://github.com/multiversx/mx-chain-go/pull/5291)

Improved trie sync process with parallelization. This is achieved by syncing the main 
trie and data tries in parallel.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes


### Relevant PRs:
- [#5226](https://github.com/multiversx/mx-chain-go/pull/5226) - Sync data tries while syncing main trie and avoid iterating main trie twice by sending leaf nodes from main tries to a leaves channel
- [#5239](https://github.com/multiversx/mx-chain-go/pull/5239) - Added unit tests for state/syncer package

## 13. VM v1.5 [#4789](https://github.com/multiversx/mx-chain-go/pull/4789)

This feature integrates the VM v1.5 and the smart contract processor v2. The following set of features become available:

**Multi-async on a single level:**\
In a single-level multi-async we do not break the execution of scA when scA makes an asyncCall (in v1 we stopped the execution of scA, so asyncCall is always the last
thing scA does in currently deployed and developed SCs), we only register the requests to the VM to make an async call after the execution ends.
This registration saves the asyncCall info into a VM internal structure, plus a set of information persisted in the storage of the SC under protected keys
(SC can read these keys, but cannot forcefully write under those, only by using the asyncCalls). Persisted information is cleaned up from SC trie after the
asyncCalls and callBacks are concluded.
scA registers a number of asyncCalls and finishes its execution. The VM iterates on the registered asyncCalls and calls them in the order they were created.
If one execution is intrashard, it will execute synchronously, in the case of cross-shard calls, those will be propagated as a smart contract result cross-shard,
and a callback is called through a callback SCR. The function which should be called back is registered and persisted in the storage, so the VM will read from
storage according to the unique IDs of the asyncCalls and get all the necessary information needed for the callback.
Introduced a limitation of multi-level asynchronous calls. AsyncCall from AsyncCall is not allowed. AsyncCall from callback is not allowed.

**ManagedBigFloats:**\
Introduced a new API that targets bigFloats. Safe math with bigFloats, using standardized GO libraries.

**ManagedMap:**\
Introduced a new API to support managed Maps for SC developers. Simple to use, all map features included.

**BackTransfers:**\
We found that we can help developers by changing some of the paradigms regarding SC to SC execution with payments. On the old VM if parentSC called childSC and childSC
transferred back tokens to parents it must have called a “deposit” function and parentSC had to register that deposit into a storage. After that childSC execution is finished,
we get back into the execution of parentSC and we have to read from storage what kind of transfers the child did towards itself.
Another problem with this is if parentSC is not payable by SC and childSC does not call deposit, only transfers, the execution will fail. This is even more complicated in
asynchronous calls, and we found that if childSC is not well written, a malicious parentSC can make it problematic for the childSC. One example was the liquid staking contracts,
where unbonded funds could get lost because malicious parentSC was not payable by SC.
So we decided to change the paradigm: When parentSC calls childSC and childSC does a set of transfers to the parent, the payable check is skipped for the parent. The check is
skipped even if the parentSC called with asyncCall the childSC and even if childSC is in another shard vs. the parentSC.
Technically speaking: All transfers without execution from childSC to parentSC are accumulated in a new structure called backTransfers. In the end, the parentSC can do the
following <payments, results> = ExecuteOnDest(child). This makes DeFi legoblocks easier to create.
The accumulated backTransfers can be read by the parentSC by calling the managedGetBackTransfers VM API.

### Impact:
* There are 2 new enable epoch definitions for this feature, called `SCProcessorV2EnableEpoch` and the VM v1.5 declaration. Beside these flag we have a new round activation flag
called `DisableAsyncCallV1`  
* The gas schedule files are changed, there are 114 new gas costs added
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4531](https://github.com/multiversx/mx-chain-go/pull/4531) - Added the smart contract processor proxy
- [#4532](https://github.com/multiversx/mx-chain-go/pull/4532) - Integrated the new smart contract processor
- [#4572](https://github.com/multiversx/mx-chain-go/pull/4572) - Fixed some tests that need flags which do not exists in VM 1.5 and in smart contract processor v2
- [#4584](https://github.com/multiversx/mx-chain-go/pull/4584) - Added testProcessorProxy to be used in tests
- [#4560](https://github.com/multiversx/mx-chain-go/pull/4560) - Minor fixes on the smart contract processor proxy
- [#4594](https://github.com/multiversx/mx-chain-go/pull/4594) - Refactored the failure processing calling code, by moving it as high as possible in the calling hierarchy.
- [#4666](https://github.com/multiversx/mx-chain-go/pull/4666) - Disable async calls for smart contract processor v1 starting from a particular round
- [#4972](https://github.com/multiversx/mx-chain-go/pull/4972) - Updated the blockchain hook on the node (+ similar changes on VM)
- [#5013](https://github.com/multiversx/mx-chain-go/pull/5013) - Added a new OriginalCallerAddr to VMInput that will be filled by SCRs OriginalSender
- [#5051](https://github.com/multiversx/mx-chain-go/pull/5051) - First Wasmer 2 integration
- [#5105](https://github.com/multiversx/mx-chain-go/pull/5105) - Integrated the required changes for the Wasmer 2
- [#5163](https://github.com/multiversx/mx-chain-go/pull/5163) - Added integration test for the async call feature
- [#5219](https://github.com/multiversx/mx-chain-go/pull/5219) - Delete the smart contract result if the transaction is intra shard and failed
- [#5266](https://github.com/multiversx/mx-chain-go/pull/5266) - Added new contracts to be used in the integration tests
- [#5253](https://github.com/multiversx/mx-chain-go/pull/5253) - Added an internal index for the executed smart contract results
- [#5326](https://github.com/multiversx/mx-chain-go/pull/5326) - Fixes after merge
- [#5353](https://github.com/multiversx/mx-chain-go/pull/5353) - Integrated new VM library, added new gas costs
- [#5427](https://github.com/multiversx/mx-chain-go/pull/5427) - Implemented the no-payable check in VM for the back-transfers feature
- [#5632](https://github.com/multiversx/mx-chain-go/pull/5632) - Integrated new VM v1.5.12 library, minor governance system fix
- [#5736](https://github.com/multiversx/mx-chain-go/pull/5736) - Integrated new VM v1.5.21 library

## 14. State package refactor [#5334](https://github.com/multiversx/mx-chain-go/pull/5334)
Moved the accounts implementations in their own package. Removed duplicated code and increased code readability.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#5295](https://github.com/multiversx/mx-chain-go/pull/5295) - Removed baseAccount from peerAccount and added trackable data trie directly in user account
- [#5329](https://github.com/multiversx/mx-chain-go/pull/5329) - Moved the user and peer account in a separate package
- [#5361](https://github.com/multiversx/mx-chain-go/pull/5361) - Moved the data trie tracker in a separate package, and removed NewEmptyPeerAccount() function
- [#5366](https://github.com/multiversx/mx-chain-go/pull/5366) - Fixes after the refactoring work: changed an `if` condition, added unit tests

## 15. Full archive refactor [#5345](https://github.com/multiversx/mx-chain-go/pull/5345)

This feature is a complete refactoring for the full archive solution. The old solution was based on the idea that full archive nodes would still
connect on the p2p network by overriding the sharding counters but in practice, it did not perform well. This new solution relies on a secondary,
optional p2p network on which only the full archive nodes will join. Since this network will mostly contain full archive nodes, the connection
between the nodes and the request-response cycles will be optimized.

### Impact:
* No activation epoch
* The `p2p.toml` file has been updated (removed the `[AdditionalConnections]` section) and the fullArchiveP2P.toml has been added
* There are 2 new optional binary flags added `full-archive-p2p-config` and `full-archive-port`
* No API endpoints changes

### Relevant PRs:
- [#5330](https://github.com/multiversx/mx-chain-go/pull/5330) - Handled multiple network messengers
- [#5332](https://github.com/multiversx/mx-chain-go/pull/5332) - Updated the heartbeatV2Components to send the info on the second network
- [#5349](https://github.com/multiversx/mx-chain-go/pull/5349) - Added an integration test on full archive network
- [#5337](https://github.com/multiversx/mx-chain-go/pull/5337) - Created and integrated a new peer shard mapper
- [#5347](https://github.com/multiversx/mx-chain-go/pull/5347) - Updated the request-resolve mechanism should to include the full archive network
- [#5365](https://github.com/multiversx/mx-chain-go/pull/5365) - Updated mx-chain-communication-go library to the latest version which has the source messenger as parameter on ProcessReceivedMessage function
- [#5374](https://github.com/multiversx/mx-chain-go/pull/5374) - Refactored the code to use only one peers rating handlers for both p2p networks
- [#5379](https://github.com/multiversx/mx-chain-go/pull/5379) - Fixed failing tests
- [#5425](https://github.com/multiversx/mx-chain-go/pull/5425) - New version for the mx-chain-communication-go library
- [#5435](https://github.com/multiversx/mx-chain-go/pull/5435) - Integrated the new the mx-chain-communication-go library version
- [#5441](https://github.com/multiversx/mx-chain-go/pull/5441) - New version for the mx-chain-communication-go library
- [#5446](https://github.com/multiversx/mx-chain-go/pull/5446) - Proper tags
- [#5448](https://github.com/multiversx/mx-chain-go/pull/5448) - Fixed local testnet scripts
- [#5604](https://github.com/multiversx/mx-chain-go/pull/5604) - Added a fallback request option while syncing old epochs data

## 16. VM-query with block coordinates [#5512](https://github.com/multiversx/mx-chain-go/pull/5512)

Added support for vm query execution on the chain state at the provided block nonce and block hash

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* The `/vm-values/query` can now receive 2 optional parameters `blockNonce` or `blockHash` and the query process will try to run the vm query on the state of the provided block coordinates

### Relevant PRs:
- [#5447](https://github.com/multiversx/mx-chain-go/pull/5447) - Added block coordinates option on vm query
- [#5528](https://github.com/multiversx/mx-chain-go/pull/5528) - Added missing line to fix long tests

## 17. Logs & events changes [#5490](https://github.com/multiversx/mx-chain-go/pull/5490)

Currently, we do not have information of how ONE contract calls another contract intra-shard. All we have are token operations.
Intra-shard token operations are taken from log/events on ESDTTransfer/ESDTNFTTransfer/MultiESDTNFTTransfer and transferValueOnly log/events.
So we do not know whether one contract calls another using ExecuteOnDest/ExecuteOnSame/AsyncCall or what function and how it is called
(if there are logs generated by the underlying SC then we know - but that is not standard).
The proposed changes involve changing the logs/events structure as following:

**For transfer EGLD or for 0 token transfers:**
```
Identifier = transferValueOnly
Address = caller
Topic = valueAsBytes, destination
Data = [ExecuteOnDestContext, vmInput.Function, vmInput.Arguments]
```

**For ESDT/NFT/Multi Transfers:**
```
Identifier = ESDTTransfer/ESDTNFTTransfer/MultiESDTNFTTransfer
Address = sender
Topics = LIST<tokenID, nonceAsBytes, valueAsBytes>, destination
Data = [ExecuteOnDestContext, vmInput.Function, vmInput.Arguments] 
```

### Impact:
* There is one enable epoch definition for this feature, called `ScToScLogEventEnableEpoch`
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#5234](https://github.com/multiversx/mx-chain-go/pull/5234) - Integrated the new libraries for the new logs structure
- [#5394](https://github.com/multiversx/mx-chain-go/pull/5394) - Added activation flag for the new logs structure
- [#5510](https://github.com/multiversx/mx-chain-go/pull/5510) - Integrated the v2 version of the MultiESDTNFTTransfer log event
- [#5530](https://github.com/multiversx/mx-chain-go/pull/5530) - Proper tags for libaries
- [#5577](https://github.com/multiversx/mx-chain-go/pull/5577) - Integrated further logs & events fixes

## 18. Transaction execution ordering refactor [#4918](https://github.com/multiversx/mx-chain-go/pull/4918)

The grouping of transactions in the block give precedence to source and destination shards (transactions are 
grouped into miniblocks, by the sender and destination shards, each miniblock having only one shard as source 
for the transactions inside (sender shard) and one shard as destination for the transactions inside (destination shard)).
This is useful for the cross shard interactions to be optimized and correctly tracked by the metachain nodes,
but does not provide details about the transactions execution order.

Currently, the transactions execution order is re-computed after the actual execution and fed into the 
outport driver, which serves clients such as indexers or notifiers. In some cases, e.g. multiple smart 
contract results generated by the same transaction, that need to be executed cross shard in the same 
destination shard, the execution order for these individual SCRs cannot be correctly determined after 
execution and all will be treated as a batch and receive the same order.

The execution order of SCRs is currently estimated (post-processing) rather than taken from execution, 
which makes it less efficient and in some cases is more prone to errors. The SCRs ordering will also become 
available from the execution component directly, together with the execution order of the other transactions 
and integrate the ordering into the outport driver.

This feature creates an ordered collection component that can be used during the transactions execution and 
collect the transactions and SCRs in their correct execution order. This feature introduces this component 
and its integration.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#4891](https://github.com/multiversx/mx-chain-go/pull/4891) - Created an ordered collection component that can be used during the transactions execution and collect the transactions and SCRs in their correct execution order.
- [#4914](https://github.com/multiversx/mx-chain-go/pull/4914) - Started the integration of the collection component
- [#4937](https://github.com/multiversx/mx-chain-go/pull/4937) - Filling of execution order in outport data provider needs to make use of to the new execution order component instead of the old one.
- [#5183](https://github.com/multiversx/mx-chain-go/pull/5183) - Fixed some problems with the transaction execution order component
- [#5525](https://github.com/multiversx/mx-chain-go/pull/5525) - Take and store the SCR execution order after processing
- [#5565](https://github.com/multiversx/mx-chain-go/pull/5565) - Added rewards transaction ordering on rewards creation in metachain

# Smaller features or fixes

### Pull requests with external impact (activation epoch, configuration file changes, binary flags changes and/or API endpoint changes):

- [#4719](https://github.com/multiversx/mx-chain-go/pull/4719) - Renamed some of the variables and added comments because some of the hb v2 config variables used to have vague names

   **Impact:** Renamed the options in the `config.file`, in the `[HeartbeatV2]` section


- [#4582](https://github.com/multiversx/mx-chain-go/pull/4582) - Added separate cli flags for logs and db paths
  
    **Impact:** Added 2 optional binary flags: `db-path` and `logs-path`


- [#5012](https://github.com/multiversx/mx-chain-go/pull/5012) - Fixed the value returned by the /transaction/pool API route

    **Impact:** On the `/transaction/pool?by-sender=erd1...&fields=value` API endpoint the 
value is now a string and should contain the data as a number in base 10 format


- [#4585](https://github.com/multiversx/mx-chain-go/pull/4585) - Added binary flag for the snashot-less node operation
  
   related with [#5428](https://github.com/multiversx/mx-chain-go/pull/5428) that reverted the `SnapshotsEnabled` configuration option removal
   **Impact:** Added 1 optional new binary flag: `snapshots-enabled`


- [#5071](https://github.com/multiversx/mx-chain-go/pull/5071) - Added new configuration option fo the vm-query execution delay

  **Impact:** Added a new configuration option in the `config.toml` file, section `[WebServerAntiflood]`


- [#4928](https://github.com/multiversx/mx-chain-go/pull/4928) - Added new endpoint in delegation manager which can handle calling claim/redelegate on multiple delegation contracts.
  
    **Impact:** There is a new enable epoch definition for this feature, called `MultiClaimOnDelegationEnableEpoch`
 

- [#5171](https://github.com/multiversx/mx-chain-go/pull/5171) - Sorted SCRs by the index they were created instead of hashes
  
    **Impact:** There is a new enable epoch definition for this feature, called `KeepExecOrderOnCreatedSCRsEnableEpoch`


- [#5204](https://github.com/multiversx/mx-chain-go/pull/5204) - Repopulate tokens supplies database
  
    **Impact:** Added 1 optional binary flag: `repopulate-tokens-supplies` that will start the recompute process after the bootstrap has been completed


- [#5229](https://github.com/multiversx/mx-chain-go/pull/5229) - Added the block processing cutoff feature able to stop a node running in debugging mode in certain conditions
  
    **Impact:** There are changes in the `prefs.toml` file as there is a new optional section `[BlockProcessingCutoff]` to be used with this feature


- [#5245](https://github.com/multiversx/mx-chain-go/pull/5245) - Added a consistent tokens values length check
  
    **Impact:** There is a new enable epoch definition for this feature, called `ConsistentTokensValuesLengthCheckEnableEpoch`


- [#5230](https://github.com/multiversx/mx-chain-go/pull/5230) - Added a fix for the change owner functionality on the delegation system SC to update the owner field in the delegation contract
  
    **Impact:** There is a new enable epoch definition for this feature, called `FixDelegationChangeOwnerOnAccountEnableEpoch`


- [#5426](https://github.com/multiversx/mx-chain-go/pull/5426) - Minor config changes
    
    **Impact:** The configuration files were updated (spacing issues and comments updates)


- [#4804](https://github.com/multiversx/mx-chain-go/pull/4804) - Added a new gas computation model for data trie loads based on the depth of the trie

    **Impact:** There is a new enable epoch definition for this feature, called `DynamicGasCostForDataTrieStorageLoadEnableEpoch`, 
the gas schedule files are changed, there are 3 new coefficients used to compute the gas cost based on the trie depth


- [#5589](https://github.com/multiversx/mx-chain-go/pull/5589) - Fixed an inconsistency for an NFT collection that happened after
the call to `stopNFTCreate` builtin-function

  **Impact:** There is a new enable epoch definition for this fix, called `NFTStopCreateEnableEpoch`


- [#5667](https://github.com/multiversx/mx-chain-go/pull/5667) - Fixed change owner address for a smartcontract when it is called
cross shard.

  **Impact:** There is a new enable epoch definition for this fix, called `ChangeOwnerAddressCrossShardThroughSCEnableEpoch`


- [#5711](https://github.com/multiversx/mx-chain-go/pull/5711) - Add resource limiter config in p2p.toml files

  **Impact:** There are changes in the p2p.toml file as there is a new section [Node.ResourceLimiter] to be used with this feature


- [#5713](https://github.com/multiversx/mx-chain-go/pull/5713) - Fixed the `SaveKeyValue` builtin function cost computation.

  **Impact:** There is a new enable epoch definition for this fix, called `FixGasRemainingForSaveKeyValueBuiltinFunctionEnableEpoch`


### Pull requests with no external impact:
- [#4656](https://github.com/multiversx/mx-chain-go/pull/4656) - Changed the trie traversing strategy to DFS (Depth First Search)
- [#4779](https://github.com/multiversx/mx-chain-go/pull/4779) - Added a benchmark that measures trie load times based on how deep the trie is
- [#4911](https://github.com/multiversx/mx-chain-go/pull/4911) - Integrated new mx-chain-p2p-go v1.0.11 library
- [#4976](https://github.com/multiversx/mx-chain-go/pull/4976) - Minor fixes in termui: termui logs window scrolled heretically when large log lines needed to be displayed
- [#4984](https://github.com/multiversx/mx-chain-go/pull/4984) - Refactored the trie mutex operations in order to optimize parallel trie accesses
- [#5018](https://github.com/multiversx/mx-chain-go/pull/5018) - Fixed the import-db process in epoch 3
- [#5006](https://github.com/multiversx/mx-chain-go/pull/5006) - Fixed an edge case bug in the block creation function
- [#4980](https://github.com/multiversx/mx-chain-go/pull/4980) - Fixed a bug on the erd_num_transactions_processed affecting the metachain nodes
- [#5037](https://github.com/multiversx/mx-chain-go/pull/5037) - Integrated new mx-chain-p2p-go v1.0.13 library
- [#4969](https://github.com/multiversx/mx-chain-go/pull/4969) - Added extra logs on the enable epochs handler component
- [#4944](https://github.com/multiversx/mx-chain-go/pull/4944) - Improve handlers sorting on epoch change subscriber
- [#5172](https://github.com/multiversx/mx-chain-go/pull/5172) - To increase consistency in system smart contract we should return error on case of all user errors. Otherwise it is hard to make integrations of these functions into other SCs.
- [#5139](https://github.com/multiversx/mx-chain-go/pull/5139) - Added unit tests for factory/bootstrap package
- [#5145](https://github.com/multiversx/mx-chain-go/pull/5145) - Added unit tests for factory/core, factory/crypto, factory/data + small fixes
- [#5152](https://github.com/multiversx/mx-chain-go/pull/5152) - Added unit tests for factory/heartbeat, factory/network, factory/state
- [#5166](https://github.com/multiversx/mx-chain-go/pull/5166) - Added unit tests for factory/status, factory/statusCore + small fixes
- [#5168](https://github.com/multiversx/mx-chain-go/pull/5168) - Added unit tests for factory/consensus
- [#5175](https://github.com/multiversx/mx-chain-go/pull/5175) - Added unit tests for api/gin + api/groups/addressGroup
- [#5184](https://github.com/multiversx/mx-chain-go/pull/5184) - Added unit tests for networkGroup, nodeGroup, proofGroup, transactionGroup, validatorGroup, vmValuesGroup
- [#5205](https://github.com/multiversx/mx-chain-go/pull/5205) - Fixed heartbeat unit tests
- [#5178](https://github.com/multiversx/mx-chain-go/pull/5178) - Added unit tests for api/groups/blockGroup, hardforkGroup, internalBlockGroup
- [#5214](https://github.com/multiversx/mx-chain-go/pull/5214) - Minor fix in termui: display if the node is in full archive mode
- [#5173](https://github.com/multiversx/mx-chain-go/pull/5173) - Added unit tests for factory/consensus
- [#5235](https://github.com/multiversx/mx-chain-go/pull/5235) - Added unit tests for trie package + some on update package
- [#5237](https://github.com/multiversx/mx-chain-go/pull/5237) - Added unit tests on facade package
- [#5244](https://github.com/multiversx/mx-chain-go/pull/5244) - Proper release for mx-chain-core-go
- [#5201](https://github.com/multiversx/mx-chain-go/pull/5201) - Complete refactor of the transaction simulation components in order to avoid sending the same AccountsDB instance used in processing
- [#5276](https://github.com/multiversx/mx-chain-go/pull/5276) - Proper release for mx-chain-vm libraries
- [#5267](https://github.com/multiversx/mx-chain-go/pull/5267) - Removed mx-chain-p2p-go dependency and integrated the new version of the mx-chain-communication-go package
- [#5177](https://github.com/multiversx/mx-chain-go/pull/5177) - Added an integration test that covers multiple situations of multi tokens transfers via relayed v2 mechanism
- [#5213](https://github.com/multiversx/mx-chain-go/pull/5213) - Added unit tests for dataRetriever
- [#5283](https://github.com/multiversx/mx-chain-go/pull/5283) - Integrated the new mx-chain-es-indexer-go version and fixed a small bug in the fee computation
- [#5294](https://github.com/multiversx/mx-chain-go/pull/5294) - Added unit tests on common package
- [#5301](https://github.com/multiversx/mx-chain-go/pull/5301) - Added unit tests on genesis package
- [#5304](https://github.com/multiversx/mx-chain-go/pull/5304) - Updated the crypto library mainly to bring support for ARM M1 (and other architectures)
- [#5331](https://github.com/multiversx/mx-chain-go/pull/5331) - Save the root hash of the snapshot in storage even if the snapshot is skipped
- [#4967](https://github.com/multiversx/mx-chain-go/pull/4967) - Refactored the `/transaction/cost/` API endpoint to include the generated logs
- [#5397](https://github.com/multiversx/mx-chain-go/pull/5397) - Changed the condition when the log event with the identifier `CompletedTxEvent` is generated for a value transfer through a smart contract result
- [#5415](https://github.com/multiversx/mx-chain-go/pull/5415) - Integrated the v1.4.6 version of the mx-chain-es-indexer-go
- [#5430](https://github.com/multiversx/mx-chain-go/pull/5430) - Intrashard miniblocks are provided to the outport drivers
- [#5442](https://github.com/multiversx/mx-chain-go/pull/5442) - Added the shard ID in all the outport structures
- [#5488](https://github.com/multiversx/mx-chain-go/pull/5488) - Updated all libraries to use go compiler v1.20
- [#5381](https://github.com/multiversx/mx-chain-go/pull/5381) - When starting a snapshot, wait for the storer to change to the new epoch, and only then continue with the snapshot process
- [#5468](https://github.com/multiversx/mx-chain-go/pull/5468) - Extended the log event that is generated when a smart contract is deployed or upgraded with an extra topic containing the codeHash
- [#5524](https://github.com/multiversx/mx-chain-go/pull/5524) - Fixed data race condition in unit tests
- [#5522](https://github.com/multiversx/mx-chain-go/pull/5522) - Import-DB process fixes: call the `trieStorageManager.ExitPruningBufferingMode()` in the `earlySnapshotCompletion` function
- [#5540](https://github.com/multiversx/mx-chain-go/pull/5540) - Added 300 seconds worth of gas limit for the API vm-queries
- [#5544](https://github.com/multiversx/mx-chain-go/pull/5544) - Extended the search for the full history resolver in case the epoch is wrongly provided
- [#5546](https://github.com/multiversx/mx-chain-go/pull/5546) - Linter fixes
- [#5509](https://github.com/multiversx/mx-chain-go/pull/5509) - Update library version for the syndtr/goleveldb library
- [#5595](https://github.com/multiversx/mx-chain-go/pull/5595) - Fixed the edge-case that a snapshot was started twice
- [#5613](https://github.com/multiversx/mx-chain-go/pull/5613) - Added round in hyperblock transactions
- [#5469](https://github.com/multiversx/mx-chain-go/pull/5469) - Updated node's dockerfile
- [#5618](https://github.com/multiversx/mx-chain-go/pull/5618) - Adjust `account not found` error when sending a transaction
- [#5651](https://github.com/multiversx/mx-chain-go/pull/5651) - Added transaction hash test example with guardian
- [#5648](https://github.com/multiversx/mx-chain-go/pull/5648) - Changed `MaxTxNonceDeltaAllowed` constant
- [#5358](https://github.com/multiversx/mx-chain-go/pull/5358) - Added extra test validators keys
- [#5464](https://github.com/multiversx/mx-chain-go/pull/5464) - Integrated the new NTP library v1.3.0
- [#5176](https://github.com/multiversx/mx-chain-go/pull/5176) - Improve requesting of missing miniblocks where the destination is self
- [#5655](https://github.com/multiversx/mx-chain-go/pull/5655) - Fixed create release workflow
- [#5659](https://github.com/multiversx/mx-chain-go/pull/5659) - Update guardians tx construction example
- [#5668](https://github.com/multiversx/mx-chain-go/pull/5668) - Integrated the new mx-chain-crypto-go library v1.2.9
- [#5686](https://github.com/multiversx/mx-chain-go/pull/5686) - Reference mx-chain-core-go version with removed omitempty for guarded API field
- [#5701](https://github.com/multiversx/mx-chain-go/pull/5701) - Fixed web server closing when the web server is disabled
- [#5695](https://github.com/multiversx/mx-chain-go/pull/5695) - Added extra checks for async arguments to avoid panic
- [#5710](https://github.com/multiversx/mx-chain-go/pull/5710) - Fixed the overridable configs when dealing with int32 parameters
- [#5688](https://github.com/multiversx/mx-chain-go/pull/5688) - Integrated the new mx-chain-communication-go v1.0.12 and mx-chain-es-indexer-go v1.4.16 libraries
- [#5733](https://github.com/multiversx/mx-chain-go/pull/5733) - Added `randSeed` and `prevRandSeed` on the APIBlock structure
- [#5746](https://github.com/multiversx/mx-chain-go/pull/5746) - Added "burn role for all" log event
- [#5749](https://github.com/multiversx/mx-chain-go/pull/5749) - Handle notifier revert data with new data payload structure 
- [#5761](https://github.com/multiversx/mx-chain-go/pull/5761) - Fixed forgotten mutexes Unlock calls
- [#5765](https://github.com/multiversx/mx-chain-go/pull/5765) - Updated VMs libraries
- [#5764](https://github.com/multiversx/mx-chain-go/pull/5764) - Fix snapshots manager

# Merges

### Note:
Starting from February 2023, the old `rc/v1.5.0` branch was renamed to `rc/v1.6.0` to allow the guardians feature 
to be the single feature merged in the new `rc/v1.5.0` branch

### PRs:
- [#4317](https://github.com/multiversx/mx-chain-go/pull/4317) - Related to mock-contracts
- [#4372](https://github.com/multiversx/mx-chain-go/pull/4372) - Part of the feat/multikey
- [#4401](https://github.com/multiversx/mx-chain-go/pull/4401) - Part of the feat/multikey
- [#4445](https://github.com/multiversx/mx-chain-go/pull/4445) - Part of the feat/multikey
- [#4458](https://github.com/multiversx/mx-chain-go/pull/4458) - Part of the feat/multikey
- [#4465](https://github.com/multiversx/mx-chain-go/pull/4465) - Part of the feat/optimise-consensus-sigcheck
- [#4472](https://github.com/multiversx/mx-chain-go/pull/4472) - Related to branch blacklist-invalid-signers
- [#4483](https://github.com/multiversx/mx-chain-go/pull/4483) - Part of the feat/vm1.5
- [#4501](https://github.com/multiversx/mx-chain-go/pull/4501) - Part of the feat/optimise-consensus-sigcheck
- [#4509](https://github.com/multiversx/mx-chain-go/pull/4509) - Related to branch pseudo-slashing-for-invalid-signers
- [#4524](https://github.com/multiversx/mx-chain-go/pull/4524) - Part of the feat/multikey
- [#4528](https://github.com/multiversx/mx-chain-go/pull/4528) - Part of the feat/multikey
- [#4557](https://github.com/multiversx/mx-chain-go/pull/4557) - Related to branch pseudo-slashing-for-invalid-signers
- [#4598](https://github.com/multiversx/mx-chain-go/pull/4598) - Related to branch blacklist-invalid-signers
- [#4615](https://github.com/multiversx/mx-chain-go/pull/4615) - Part of the feat/multikey
- [#4619](https://github.com/multiversx/mx-chain-go/pull/4619) - Related to branch blacklist-invalid-signers
- [#4628](https://github.com/multiversx/mx-chain-go/pull/4628) - Part of the feat/optimise-consensus-sigcheck
- [#4651](https://github.com/multiversx/mx-chain-go/pull/4651) - Part of the feat/vm1.5
- [#4652](https://github.com/multiversx/mx-chain-go/pull/4652) - Part of the feat/balance-data-tries
- [#4653](https://github.com/multiversx/mx-chain-go/pull/4653) - Related to branch cli-flag-for-snapshots-enabled
- [#4665](https://github.com/multiversx/mx-chain-go/pull/4665) - Part of the feat/optimise-consensus-sigcheck
- [#4677](https://github.com/multiversx/mx-chain-go/pull/4677) - Part of the feat/multikey
- [#4720](https://github.com/multiversx/mx-chain-go/pull/4720) - Part of the feat/sync-missing-trie-nodes
- [#4739](https://github.com/multiversx/mx-chain-go/pull/4739) - Part of the feat/multikey
- [#4752](https://github.com/multiversx/mx-chain-go/pull/4752) - Part of the feat/refactor_resolvers
- [#4759](https://github.com/multiversx/mx-chain-go/pull/4759) - Part of the feat/optimise-consensus-sigcheck
- [#4762](https://github.com/multiversx/mx-chain-go/pull/4762) - Related to branch cli-flags-for-logs-and-db-paths
- [#4763](https://github.com/multiversx/mx-chain-go/pull/4763) - Related to branch cli-flag-for-snapshots-enabled
- [#4766](https://github.com/multiversx/mx-chain-go/pull/4766) - Part of the feat/optimise-consensus-sigcheck
- [#4771](https://github.com/multiversx/mx-chain-go/pull/4771) - Part of the feat/multikey
- [#4776](https://github.com/multiversx/mx-chain-go/pull/4776) - Part of the feat/multikey
- [#4778](https://github.com/multiversx/mx-chain-go/pull/4778) - Part of the feat/pubkeyconverter-refactor
- [#4790](https://github.com/multiversx/mx-chain-go/pull/4790) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4792](https://github.com/multiversx/mx-chain-go/pull/4792) - Part of the feat/refactor_resolvers
- [#4793](https://github.com/multiversx/mx-chain-go/pull/4793) - Part of the feat/pubkeyconverter-refactor
- [#4812](https://github.com/multiversx/mx-chain-go/pull/4812) - Part of the feat/pubkeyconverter-refactor
- [#4813](https://github.com/multiversx/mx-chain-go/pull/4813) - Part of the feat/multikey
- [#4834](https://github.com/multiversx/mx-chain-go/pull/4834) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4837](https://github.com/multiversx/mx-chain-go/pull/4837) - Part of the feat/balance-data-tries
- [#4840](https://github.com/multiversx/mx-chain-go/pull/4840) - Part of the feat/fix-peers-rating-handler
- [#4880](https://github.com/multiversx/mx-chain-go/pull/4880) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4890](https://github.com/multiversx/mx-chain-go/pull/4890) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4892](https://github.com/multiversx/mx-chain-go/pull/4892) - Part of the feat/balance-data-tries
- [#4901](https://github.com/multiversx/mx-chain-go/pull/4901) - Part of the feat/fix-peers-rating-handler
- [#4905](https://github.com/multiversx/mx-chain-go/pull/4905) - Part of the feat/multikey
- [#4909](https://github.com/multiversx/mx-chain-go/pull/4909) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4926](https://github.com/multiversx/mx-chain-go/pull/4926) - Part of the feat/balance-data-tries
- [#4939](https://github.com/multiversx/mx-chain-go/pull/4939) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4942](https://github.com/multiversx/mx-chain-go/pull/4942) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4946](https://github.com/multiversx/mx-chain-go/pull/4946) - Part of the feat/vm1.5
- [#4955](https://github.com/multiversx/mx-chain-go/pull/4955) - Part of the feat/sync-missing-trie-nodes
- [#4960](https://github.com/multiversx/mx-chain-go/pull/4960) - Part of the feat/balance-data-tries
- [#4982](https://github.com/multiversx/mx-chain-go/pull/4982) - Merge rc/v1.4.0 -> rc/v1.6.0
- [#4985](https://github.com/multiversx/mx-chain-go/pull/4985) - Part of the feat/fix-peers-rating-handler
- [#5000](https://github.com/multiversx/mx-chain-go/pull/5000) - Merge master -> rc/v1.6.0
- [#5007](https://github.com/multiversx/mx-chain-go/pull/5007) - Part of the feat/multikey
- [#5011](https://github.com/multiversx/mx-chain-go/pull/5011) - Part of the feat/sharded-persister
- [#5023](https://github.com/multiversx/mx-chain-go/pull/5023) - Merge master -> rc/v1.6.0
- [#5025](https://github.com/multiversx/mx-chain-go/pull/5025) - Part of the feat/pubkeyconverter-refactor
- [#5033](https://github.com/multiversx/mx-chain-go/pull/5033) - Part of the feat/multikey
- [#5040](https://github.com/multiversx/mx-chain-go/pull/5040) - Part of the feat/vm1.5
- [#5042](https://github.com/multiversx/mx-chain-go/pull/5042) - Merge rc/v1.5.0 -> rc/v1.6.0
- [#5047](https://github.com/multiversx/mx-chain-go/pull/5047) - Part of the feat/balance-data-tries
- [#5049](https://github.com/multiversx/mx-chain-go/pull/5049) - Part of the feat/multikey
- [#5053](https://github.com/multiversx/mx-chain-go/pull/5053) - Part of the feat/fix-peers-rating-handler
- [#5054](https://github.com/multiversx/mx-chain-go/pull/5054) - Part of the feat/fix-peers-rating-handler
- [#5058](https://github.com/multiversx/mx-chain-go/pull/5058) - Part of the feat/sync-missing-trie-nodes
- [#5066](https://github.com/multiversx/mx-chain-go/pull/5066) - Part of the feat/pubkeyconverter-refactor
- [#5084](https://github.com/multiversx/mx-chain-go/pull/5084) - Part of the feat/pubkeyconverter-refactor
- [#5085](https://github.com/multiversx/mx-chain-go/pull/5085) - Merge rc/v1.5.0 -> rc/v1.6.0
- [#5086](https://github.com/multiversx/mx-chain-go/pull/5086) - Part of the feat/vm1.5
- [#5100](https://github.com/multiversx/mx-chain-go/pull/5100) - Part of the feat/pubkeyconverter-refactor
- [#5104](https://github.com/multiversx/mx-chain-go/pull/5104) - Part of the feat/pubkeyconverter-refactor
- [#5116](https://github.com/multiversx/mx-chain-go/pull/5116) - Part of the feat/outport-refactor
- [#5125](https://github.com/multiversx/mx-chain-go/pull/5125) - Part of the feat/fix-peers-rating-handler
- [#5143](https://github.com/multiversx/mx-chain-go/pull/5143) - Part of the feat/outport-refactor
- [#5147](https://github.com/multiversx/mx-chain-go/pull/5147) - Part of the feat/sync-missing-trie-nodes
- [#5151](https://github.com/multiversx/mx-chain-go/pull/5151) - Part of the feat/balance-data-tries
- [#5179](https://github.com/multiversx/mx-chain-go/pull/5179) - Part of the feat/transaction-ordering-on-execution
- [#5186](https://github.com/multiversx/mx-chain-go/pull/5186) - Merge rc/v1.5.0 -> rc/v1.6.0
- [#5193](https://github.com/multiversx/mx-chain-go/pull/5193) - Part of the feat/balance-data-tries
- [#5195](https://github.com/multiversx/mx-chain-go/pull/5195) - Part of the feat/vm1.5
- [#5212](https://github.com/multiversx/mx-chain-go/pull/5212) - Part of the feat/sharded-persister
- [#5220](https://github.com/multiversx/mx-chain-go/pull/5220) - Part of the feat/vm1.5
- [#5228](https://github.com/multiversx/mx-chain-go/pull/5228) - Part of the feat/sharded-persister
- [#5241](https://github.com/multiversx/mx-chain-go/pull/5241) - Part of the feat/sync-missing-trie-nodes
- [#5243](https://github.com/multiversx/mx-chain-go/pull/5243) - Part of the feat/sync-missing-trie-nodes
- [#5249](https://github.com/multiversx/mx-chain-go/pull/5249) - Part of the feat/outport-refactor
- [#5251](https://github.com/multiversx/mx-chain-go/pull/5251) - Part of the feat/sync-missing-trie-nodes
- [#5263](https://github.com/multiversx/mx-chain-go/pull/5263) - Part of the feat/sharded-persister
- [#5268](https://github.com/multiversx/mx-chain-go/pull/5268) - Part of the feat/balance-data-tries
- [#5270](https://github.com/multiversx/mx-chain-go/pull/5270) - Part of the feat/sync-missing-trie-nodes
- [#5272](https://github.com/multiversx/mx-chain-go/pull/5272) - Part of the feat/sharded-persister
- [#5279](https://github.com/multiversx/mx-chain-go/pull/5279) - Part of the feat/balance-data-tries
- [#5287](https://github.com/multiversx/mx-chain-go/pull/5287) - Part of the feat/balance-data-tries
- [#5292](https://github.com/multiversx/mx-chain-go/pull/5292) - Part of the feat/balance-data-tries
- [#5293](https://github.com/multiversx/mx-chain-go/pull/5293) - Part of the feat/trie-sync-optimizations
- [#5302](https://github.com/multiversx/mx-chain-go/pull/5302) - Part of the feat/sharded-persister
- [#5309](https://github.com/multiversx/mx-chain-go/pull/5309) - Part of the feat/trie-sync-optimizations
- [#5310](https://github.com/multiversx/mx-chain-go/pull/5310) - Merge rc/v1.5.0 -> rc/v1.6.0
- [#5325](https://github.com/multiversx/mx-chain-go/pull/5325) - Part of the feat/vm1.5
- [#5364](https://github.com/multiversx/mx-chain-go/pull/5364) - Merge rc/v1.5.0 -> rc/v1.6.0
- [#5368](https://github.com/multiversx/mx-chain-go/pull/5368) - Part of the feat/vm1.5
- [#5369](https://github.com/multiversx/mx-chain-go/pull/5369) - Part of the feat/multikey_metrics
- [#5377](https://github.com/multiversx/mx-chain-go/pull/5377) - Part of the feat/multikey_metrics
- [#5378](https://github.com/multiversx/mx-chain-go/pull/5378) - Part of the feat/multiple_p2p_messengers
- [#5382](https://github.com/multiversx/mx-chain-go/pull/5382) - Part of the feat/state-refactor
- [#5387](https://github.com/multiversx/mx-chain-go/pull/5387) - Part of the feat/state-refactor
- [#5388](https://github.com/multiversx/mx-chain-go/pull/5388) - Part of the feat/logEvents
- [#5393](https://github.com/multiversx/mx-chain-go/pull/5393) - Part of the feat/state-refactor
- [#5396](https://github.com/multiversx/mx-chain-go/pull/5396) - Part of the feat/logEvents
- [#5408](https://github.com/multiversx/mx-chain-go/pull/5408) - Merge master -> rc/v1.6.0
- [#5411](https://github.com/multiversx/mx-chain-go/pull/5411) - Part of the feat/multiple_p2p_messengers
- [#5412](https://github.com/multiversx/mx-chain-go/pull/5412) - Part of the feat/multikey_metrics
- [#5414](https://github.com/multiversx/mx-chain-go/pull/5414) - Part of the feat/multikey_metrics
- [#5416](https://github.com/multiversx/mx-chain-go/pull/5416) - Part of the feat/multiple_p2p_messengers
- [#5420](https://github.com/multiversx/mx-chain-go/pull/5420) - Part of the feat/multikey_metrics
- [#5444](https://github.com/multiversx/mx-chain-go/pull/5444) - Part of the feat/multiple_p2p_messengers
- [#5465](https://github.com/multiversx/mx-chain-go/pull/5465) - Merge master -> rc/v1.6.0
- [#5486](https://github.com/multiversx/mx-chain-go/pull/5486) - Part of the feat/transaction-ordering-on-execution
- [#5496](https://github.com/multiversx/mx-chain-go/pull/5496) - Part of the feat/logEvents
- [#5515](https://github.com/multiversx/mx-chain-go/pull/5515) - Part of the feat/vmquery_with_block_coordinates
- [#5516](https://github.com/multiversx/mx-chain-go/pull/5516) - Part of the feat/logEvents
- [#5518](https://github.com/multiversx/mx-chain-go/pull/5518) - Merge master -> rc/v1.6.0
- [#5526](https://github.com/multiversx/mx-chain-go/pull/5526) - Part of the feat/logEvents
- [#5527](https://github.com/multiversx/mx-chain-go/pull/5527) - Part of the feat/vmquery_with_block_coordinates
- [#5529](https://github.com/multiversx/mx-chain-go/pull/5529) - Part of the feat/logEvents
- [#5538](https://github.com/multiversx/mx-chain-go/pull/5538) - Part of the feat/transaction-ordering-on-execution
- [#5554](https://github.com/multiversx/mx-chain-go/pull/5554) - Part of the feat/transaction-ordering-on-execution
- [#5555](https://github.com/multiversx/mx-chain-go/pull/5555) - Part of the feat/transaction-ordering-on-execution
- [#5687](https://github.com/multiversx/mx-chain-go/pull/5687) - Merge master -> rc/v1.6.0
- [#5724](https://github.com/multiversx/mx-chain-go/pull/5724) - Merge master -> rc/v1.6.0
