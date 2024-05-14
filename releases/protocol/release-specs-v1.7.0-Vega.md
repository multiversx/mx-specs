[comment]: <> (tags/v1.7.8)

# Contents

This document explains the contents of the rc/v1.7.0 release codenamed Vega. It is split in 2 sections:
- the **features** list containing detailed insights of the feature along with the external impact and the relevant
  pull requests list
- the **smaller features and fixes** area contains the one-pull request small features or fixes along with the
  external impact details

This documentation is relevant for the `tags/v1.7.10` tag release.

# Features

## 1. Staking v4 [#4934](https://github.com/multiversx/mx-chain-go/pull/4467)

Staking v4 feature eliminates the infamous staking queue that prevented easier access to a staking position regardless the stake value. The queue was replaced with an auction mechanism that decides at each epoch change the key set that will be pushed to the waiting list based on the distributed top-up available.
The official staking-v4 docs are found [here](https://docs.multiversx.com/validators/staking-v4/)

### Impact:
* There are new enable epochs definition for this feature, called `StakeLimitsEnableEpoch`, `StakingV4Step1EnableEpoch`, `StakingV4Step2EnableEpoch` and `StakingV4Step3EnableEpoch`
* There are changes to the configuration options in the `systemSmartContractsConfig.toml` file, `[StakingSystemSCConfig]` section and a new section called `SoftAuctionConfig`
* No binary flags changes
* There is a new API endpoint called `validator/auction`

### Relevant PRs:
- [#6078](https://github.com/multiversx/mx-chain-go/pull/6078) - Fixed unstaked list on delegation when nodes are unstaked from queue
- [#6042](https://github.com/multiversx/mx-chain-go/pull/6042) - Remove all nodes from queue on the activation of staking v4
- [#6040](https://github.com/multiversx/mx-chain-go/pull/6040) - Added stake-unstake-unbond scenario + EEI fix
- [#6034](https://github.com/multiversx/mx-chain-go/pull/6034) - FIX: Warn for too low waiting list to debug
- [#5984](https://github.com/multiversx/mx-chain-go/pull/5984) - Staking for direct staked nodes - part 2
- [#6017](https://github.com/multiversx/mx-chain-go/pull/6017) - Fixed the integration test that sometimes fails
- [#5949](https://github.com/multiversx/mx-chain-go/pull/5949) - Staking for direct staked nodes - part 1
- [#6006](https://github.com/multiversx/mx-chain-go/pull/6006) - Added staking v4 scenario 11
- [#6000](https://github.com/multiversx/mx-chain-go/pull/6000) - Minor chain simulator refactor & unit tests
- [#5978](https://github.com/multiversx/mx-chain-go/pull/5978) - Unit tests for proper flags
- [#5979](https://github.com/multiversx/mx-chain-go/pull/5979) - Process proxy fix
- [#5975](https://github.com/multiversx/mx-chain-go/pull/5975) - Proper flag for GovernanceFlagInSpecificEpochOnly
- [#5974](https://github.com/multiversx/mx-chain-go/pull/5974) - Proper DelegationSmartContractFlag flag
- [#5969](https://github.com/multiversx/mx-chain-go/pull/5969) - Staking v4 nodes config provider for API calls
- [#5941](https://github.com/multiversx/mx-chain-go/pull/5941) - Merging delegation scenario
- [#5961](https://github.com/multiversx/mx-chain-go/pull/5961) - Scenarios 4, 5 and 6 for staking v4
- [#5932](https://github.com/multiversx/mx-chain-go/pull/5932) - Test that the CreateNewDelegationContract function works properly
- [#5954](https://github.com/multiversx/mx-chain-go/pull/5954) - Removed resource leaks in chainSimulator and apiResolverFactory
- [#5937](https://github.com/multiversx/mx-chain-go/pull/5937) - Staking v4 - scenario no. 2
- [#5924](https://github.com/multiversx/mx-chain-go/pull/5924) - Fix stakingV4 previous list
- [#5946](https://github.com/multiversx/mx-chain-go/pull/5946) - FIX: Duplicated public key in staking v4
- [#5952](https://github.com/multiversx/mx-chain-go/pull/5952) - FIX: Rename auction list nodes to nodes
- [#5930](https://github.com/multiversx/mx-chain-go/pull/5930) - Jail-UnJail scenario
- [#5923](https://github.com/multiversx/mx-chain-go/pull/5923) - No registration for too many key
- [#5931](https://github.com/multiversx/mx-chain-go/pull/5931) - Call chainSimulator.Close on all occasions to avoid resource leaks
- [#5926](https://github.com/multiversx/mx-chain-go/pull/5926) - Staking v4 delegation scenario 10
- [#5925](https://github.com/multiversx/mx-chain-go/pull/5925) - Fix shard is stuck problem
- [#5920](https://github.com/multiversx/mx-chain-go/pull/5920) - Staking v4 integration tests
- [#5918](https://github.com/multiversx/mx-chain-go/pull/5918) - Add auction list displayer component and disable it on API
- [#5914](https://github.com/multiversx/mx-chain-go/pull/5914) - Minor config adjustment for staking v4
- [#5915](https://github.com/multiversx/mx-chain-go/pull/5915) - System test config like scenario for sanity checks
- [#5913](https://github.com/multiversx/mx-chain-go/pull/5913) - Fix unit test
- [#5887](https://github.com/multiversx/mx-chain-go/pull/5887) - Adjusted p2p parameters for internal system testnets
- [#5912](https://github.com/multiversx/mx-chain-go/pull/5912) - FIX: Leaving node in previous config staking v4
- [#5905](https://github.com/multiversx/mx-chain-go/pull/5905) - Staking v4 bugfixes
- [#5858](https://github.com/multiversx/mx-chain-go/pull/5858) - Do not activate more nodes on stake if too many nodes
- [#5825](https://github.com/multiversx/mx-chain-go/pull/5825) - FIX: Remove enforced config protections
- [#5111](https://github.com/multiversx/mx-chain-go/pull/5111) - FIX: Low waiting list edge case in StakingV4Step2
- [#5093](https://github.com/multiversx/mx-chain-go/pull/5093) - Deterministic displayer integration tests
- [#5078](https://github.com/multiversx/mx-chain-go/pull/5078) - Config protection max, min nodes with hysteresis
- [#5061](https://github.com/multiversx/mx-chain-go/pull/5061) - Fix edge case empty waiting list
- [#4993](https://github.com/multiversx/mx-chain-go/pull/4993) - FIX: Remove warn
- [#4989](https://github.com/multiversx/mx-chain-go/pull/4989) - Remove unused IsTransferToMetaFlagEnabled
- [#4983](https://github.com/multiversx/mx-chain-go/pull/4983) - Protection check for staking v4 configs
- [#4970](https://github.com/multiversx/mx-chain-go/pull/4970) - Rename staking v4 flags to steps
- [#4961](https://github.com/multiversx/mx-chain-go/pull/4961) - Add PreviousList to peersAccount/validatorInfo/shardValidatorInfo
- [#4178](https://github.com/multiversx/mx-chain-go/pull/4178) - Auction list API endpoint v2
- [#4068](https://github.com/multiversx/mx-chain-go/pull/4068) - Auction list API endpoint
- [#4170](https://github.com/multiversx/mx-chain-go/pull/4170) - Integration tests jail/unJail
- [#4121](https://github.com/multiversx/mx-chain-go/pull/4121) - Integration tests for unStake nodes
- [#4125](https://github.com/multiversx/mx-chain-go/pull/4125) - Save nodes config
- [#4155](https://github.com/multiversx/mx-chain-go/pull/4155) - Refactor stakingDataProvider to pre-fill auction data
- [#4083](https://github.com/multiversx/mx-chain-go/pull/4083) - Soft auction list selection
- [#4132](https://github.com/multiversx/mx-chain-go/pull/4132) - Fix latency unit test
- [#4128](https://github.com/multiversx/mx-chain-go/pull/4128) - Fix broken test
- [#4073](https://github.com/multiversx/mx-chain-go/pull/4073) - Create auction list selector subcomponent
- [#4045](https://github.com/multiversx/mx-chain-go/pull/4045) - Integration tests with custom scenarios
- [#4049](https://github.com/multiversx/mx-chain-go/pull/4049) - Fix adding keys to waiting list in tests setup
- [#3993](https://github.com/multiversx/mx-chain-go/pull/3993) - Bug fixes staking v4
- [#3951](https://github.com/multiversx/mx-chain-go/pull/3951) - Integration tests staking v4
- [#3932](https://github.com/multiversx/mx-chain-go/pull/3932) - Validator statistics - save shuffled out nodes in auction list
- [#3930](https://github.com/multiversx/mx-chain-go/pull/3930) - Integrate validators info map into staking data provider
- [#3929](https://github.com/multiversx/mx-chain-go/pull/3929) - Integrate validators info map into rewards creator
- [#3926](https://github.com/multiversx/mx-chain-go/pull/3926) - Integrate validators info map into validator creator
- [#3915](https://github.com/multiversx/mx-chain-go/pull/3915) - Integrate validators info map into validators statistics
- [#3908](https://github.com/multiversx/mx-chain-go/pull/3908) - Integrate validators info map into system SC
- [#3907](https://github.com/multiversx/mx-chain-go/pull/3907) - Shard validator info handler map interface
- [#3883](https://github.com/multiversx/mx-chain-go/pull/3883) - Nodes coordinator with staking v4
- [#3866](https://github.com/multiversx/mx-chain-go/pull/3866) - Move all waiting list related code from staking.go to stakingWaitingList.go
- [#3861](https://github.com/multiversx/mx-chain-go/pull/3861) - Ignore staking queue
- [#3891](https://github.com/multiversx/mx-chain-go/pull/3891) - SystemSCs.go code split
- [#3822](https://github.com/multiversx/mx-chain-go/pull/3822) - Filter auction nodes list
- [#3818](https://github.com/multiversx/mx-chain-go/pull/3818) - Staking v4 init auction nodes list
- [#3447](https://github.com/multiversx/mx-chain-go/pull/3447) - Added stake operation limitations
- [#6114](https://github.com/multiversx/mx-chain-go/pull/6114) - POC: Distribute to waiting from auction based on leaving nodes
- [#5953](https://github.com/multiversx/mx-chain-go/pull/5953) - MaxDelegationCap tests

## 2. Enable epochs component refactor [#5419](https://github.com/multiversx/mx-chain-go/pull/5419)

This feature refactors the enable epochs component to become more extensible and avoid the unnecessary changes in the satellite repos for the mx-chain-go repo.

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:
- [#5617](https://github.com/multiversx/mx-chain-go/pull/5617) - Proper tags for satellite repos
- [#5541](https://github.com/multiversx/mx-chain-go/pull/5541) - Add check for enable epochs handler on all subcomponents + cleanup
- [#5521](https://github.com/multiversx/mx-chain-go/pull/5521) - GetActivationEpoch implementation + further refactor
- [#5520](https://github.com/multiversx/mx-chain-go/pull/5520) - Replace boolean methods from enableEpochsHandler
- [#5491](https://github.com/multiversx/mx-chain-go/pull/5491) - New methods enable epochs handle
- [#5432](https://github.com/multiversx/mx-chain-go/pull/5432) - Update enableEpochsHandler methods part 3
- [#5431](https://github.com/multiversx/mx-chain-go/pull/5431) - Update enableEpochsHandler methods part 2
- [#5424](https://github.com/multiversx/mx-chain-go/pull/5424) - Update vm-common and vms + remove old methods part 1
- [#5418](https://github.com/multiversx/mx-chain-go/pull/5418) - Updated mock and stub of enableEpochsHandler
- [#5417](https://github.com/multiversx/mx-chain-go/pull/5417) - New enable epochs handler functionality

## 3. Chain simulator [#5558](https://github.com/multiversx/mx-chain-go/pull/5558)

This feature adds the components required to create a binary that can be configured and parametrized to run like a very small-footprint blockchain. It supports sharding, full block processing, fast block generation and 100% full proxy (gateway) endpoints.
This tool can automatize testing scenarios that could, otherwise, be possible only on deployed blockchain consisting on a multiple node instances.
The integration can be used from [here](https://github.com/multiversx/mx-chain-simulator-go)

### Impact:
* No activation epoch
* No configuration files changes
* No binary flags changes
* No API endpoints changes

### Relevant PRs:

- [#6045](https://github.com/multiversx/mx-chain-go/pull/6045) - Applied custom arch config tweaks on the chain simulator
- [#6004](https://github.com/multiversx/mx-chain-go/pull/6004) - Fixed chain simulator's synced messenger to prepare the Peer field in the message
- [#5993](https://github.com/multiversx/mx-chain-go/pull/5993) - Bug-fixes for SetState chain simulator operation
- [#5990](https://github.com/multiversx/mx-chain-go/pull/5990) - More chain simulator tests part 3
- [#5985](https://github.com/multiversx/mx-chain-go/pull/5985) - More chain simulator tests part 2
- [#5986](https://github.com/multiversx/mx-chain-go/pull/5986) - Defined genesis epoch
- [#5971](https://github.com/multiversx/mx-chain-go/pull/5971) - More chain simulator tests
- [#5980](https://github.com/multiversx/mx-chain-go/pull/5980) - Chain simulator initial wallets refactor
- [#5965](https://github.com/multiversx/mx-chain-go/pull/5965) - Chain simulator test only processing node tests
- [#5964](https://github.com/multiversx/mx-chain-go/pull/5964) - Chain simulator processor tests
- [#5909](https://github.com/multiversx/mx-chain-go/pull/5909) - Skipped a test for chain simulator
- [#5907](https://github.com/multiversx/mx-chain-go/pull/5907) - Fixes for chain-simulator
- [#5850](https://github.com/multiversx/mx-chain-go/pull/5850) - Defined initial round
- [#5789](https://github.com/multiversx/mx-chain-go/pull/5789) - Multiple validators
- [#5752](https://github.com/multiversx/mx-chain-go/pull/5752) - Set entire state of an account
- [#5740](https://github.com/multiversx/mx-chain-go/pull/5740) - Bug-fixes and improvements
- [#5732](https://github.com/multiversx/mx-chain-go/pull/5732) - Bypass transaction signature
- [#5727](https://github.com/multiversx/mx-chain-go/pull/5727) - Extra endpoint for SetState operation
- [#5716](https://github.com/multiversx/mx-chain-go/pull/5716) - Allow VM queries
- [#5709](https://github.com/multiversx/mx-chain-go/pull/5709) - Initial wallet keys
- [#5699](https://github.com/multiversx/mx-chain-go/pull/5699) - Enable HTTP server option 
- [#5697](https://github.com/multiversx/mx-chain-go/pull/5697) - Fixes for chain simulator
- [#5684](https://github.com/multiversx/mx-chain-go/pull/5684) - Added & integrated manual round handler
- [#5681](https://github.com/multiversx/mx-chain-go/pull/5681) - Process blocks
- [#5677](https://github.com/multiversx/mx-chain-go/pull/5677) - Create configs
- [#5625](https://github.com/multiversx/mx-chain-go/pull/5625) - Data components and process components
- [#5584](https://github.com/multiversx/mx-chain-go/pull/5584) - Bootstrap components
- [#5578](https://github.com/multiversx/mx-chain-go/pull/5578) - Added network components in chain simulator
- [#5576](https://github.com/multiversx/mx-chain-go/pull/5576) - Crypto components
- [#5575](https://github.com/multiversx/mx-chain-go/pull/5575) - Added synced network messenger
- [#5573](https://github.com/multiversx/mx-chain-go/pull/5573) - Status Core Components and State Components
- [#5563](https://github.com/multiversx/mx-chain-go/pull/5563) - Initialize core components
- [#5552](https://github.com/multiversx/mx-chain-go/pull/5552) - TestOnlyProcessingNode - part 1
- [#6061](https://github.com/multiversx/mx-chain-go/pull/6061) - Host driver option
- [#6092](https://github.com/multiversx/mx-chain-go/pull/6092) - Integration tests multiple packages
- [#6126](https://github.com/multiversx/mx-chain-go/pull/6126) - Fixed the initialization of the chain simulator when working with 0-value activation epochs

# Smaller features or fixes

- [#6054](https://github.com/multiversx/mx-chain-go/pull/6054) - Added more files in the overridable configs options and chain simulator fixes
- [#6047](https://github.com/multiversx/mx-chain-go/pull/6047) - Uniformized the calling methods for integration tests
- [#6032](https://github.com/multiversx/mx-chain-go/pull/6032) - Fixed 2 metrics initialization in epoch 0
- [#6013](https://github.com/multiversx/mx-chain-go/pull/6013) - Add support for Apple ARM64 and Linux ARM64
- [#6026](https://github.com/multiversx/mx-chain-go/pull/6026) - Fix linter issues by removing unused methods
- [#5988](https://github.com/multiversx/mx-chain-go/pull/5988) - Fix genesis flags
- [#5813](https://github.com/multiversx/mx-chain-go/pull/5813) - Tests for requests
- [#5728](https://github.com/multiversx/mx-chain-go/pull/5728) - Remove built-in function cost handler
- [#5767](https://github.com/multiversx/mx-chain-go/pull/5767) - Fix gasUsed and fee move balance transaction
- [#5880](https://github.com/multiversx/mx-chain-go/pull/5880) - Fix /transaction/cost API endpoint route
- [#5944](https://github.com/multiversx/mx-chain-go/pull/5944) - Configurable epoch change delay
- [#5868](https://github.com/multiversx/mx-chain-go/pull/5868) - Protocol ID check in seeder
- [#5683](https://github.com/multiversx/mx-chain-go/pull/5683) - Switch to current block randomness for ordering transactions
- [#5795](https://github.com/multiversx/mx-chain-go/pull/5795) - Fix import-db resource leak
- [#5792](https://github.com/multiversx/mx-chain-go/pull/5792) - Fix metric count consensus
- [#5775](https://github.com/multiversx/mx-chain-go/pull/5775) - Context closing during snapshot bugfix
- [#5776](https://github.com/multiversx/mx-chain-go/pull/5776) - Integrated new logger version
- [#5705](https://github.com/multiversx/mx-chain-go/pull/5705) - Trie sync progress status
- [#5738](https://github.com/multiversx/mx-chain-go/pull/5738) - Move ValidatorAPIResponse structure to mx-chain-core-go
- [#5747](https://github.com/multiversx/mx-chain-go/pull/5747) - Integrated cache-less mx-core-go libs
- [#5714](https://github.com/multiversx/mx-chain-go/pull/5714) - New API endpoint /node/waiting-epochs-left/:key
- [#5707](https://github.com/multiversx/mx-chain-go/pull/5707) - Added new flag + readme for starting p2p prometheus dashboards
- [#5674](https://github.com/multiversx/mx-chain-go/pull/5674) - Use disabled snapshots manager if the snapshots are disabled
- [#5401](https://github.com/multiversx/mx-chain-go/pull/5401) - Trie storage statistics component
- [#5672](https://github.com/multiversx/mx-chain-go/pull/5672) - Remove the state checkpoint operation
- [#5675](https://github.com/multiversx/mx-chain-go/pull/5675) - Renamed stubGenerator to mockGenerator
- [#5642](https://github.com/multiversx/mx-chain-go/pull/5642) - LastSnapshot marker fix
- [#5649](https://github.com/multiversx/mx-chain-go/pull/5649) - Replace SendTxRequest with FrontendTransaction
- [#5901](https://github.com/multiversx/mx-chain-go/pull/5901) - Add missing state statistics field to config file
- [#6141](https://github.com/multiversx/mx-chain-go/pull/6141) - Fix genesis flags
- [#6108](https://github.com/multiversx/mx-chain-go/pull/6108) - Added block timestamp + scripts update for the round duration
- [#6104](https://github.com/multiversx/mx-chain-go/pull/6104) - Fixed backwards compatibility problem
- [#6122](https://github.com/multiversx/mx-chain-go/pull/6122) - Update x/crypto to v0.21.0
- [#6123](https://github.com/multiversx/mx-chain-go/pull/6123) - Add withKeys option on account
