[comment]: <> (tags/v1.8.4)

# Contents

This document explains the contents of the rc/v1.8 release codenamed Spica. It is split in 2 sections:
- the **features** list containing detailed insights of the feature along with the external impact and the relevant
  pull requests list
- the **smaller features and fixes** area contains the one-pull request small features or fixes along with the
  external impact details

This documentation is relevant for the `tags/v1.8.4` tag release.

# Features

## 1. ESDT improvements [#5821](https://github.com/multiversx/mx-chain-go/pull/5821)

The improvements implemented on ESDTs enable the dynamic NFT functionality.

### Impact:
* There are new roles added `ESDTModifyRoyalties`, `ESDTModifyCreator`, `ESDTNFTRecreate`, `ESDTNFTUpdate`, `ESDTNFTSetNewURIs`
* There is a new enable epoch definition for this feature, called `DynamicESDTEnableEpoch`
* There are changes to the configuration options in the gas schedule files, new values were added for `ESDTModifyRoyalties`, `ESDTModifyCreator`,`ESDTNFTRecreate`,`ESDTNFTUpdate`, `ESDTNFTSetNewURIs`
* No Node CLI arguments changes
* API endpoints `/:address/nft/:tokenIdentifier/nonce/:nonce` and `/:address/esdt` will now return token type as well

### Relevant PRs:
- [#5806](https://github.com/multiversx/mx-chain-go/pull/5806) - ESDT improvements integration
- [#5682](https://github.com/multiversx/mx-chain-go/pull/5682) - ESDT v2 implementations on SystemSC - dynamic tokens and tokenType
- [#6284](https://github.com/multiversx/mx-chain-go/pull/6284) - ESDT Improvements integration tests
- [#6194](https://github.com/multiversx/mx-chain-go/pull/6194) - ESDT improvements chain simulator tests and fixes
- [#6262](https://github.com/multiversx/mx-chain-go/pull/6262) - ESDT improvements integration tests part2 + fixes
- [#6250](https://github.com/multiversx/mx-chain-go/pull/6250) - Do not allow NFTs to be upgraded to dynamic
- [#6263](https://github.com/multiversx/mx-chain-go/pull/6263) - Add more testing scenarios
- [#6242](https://github.com/multiversx/mx-chain-go/pull/6242) - Add esdt token type api
- [#6220](https://github.com/multiversx/mx-chain-go/pull/6220) - Token type in altered accounts
- [#6350](https://github.com/multiversx/mx-chain-go/pull/6350) - Fix missing role + Use setSpecialRole function in tests
- [#6357](https://github.com/multiversx/mx-chain-go/pull/6357) - Dynamic roles singular
- [#6434](https://github.com/multiversx/mx-chain-go/pull/6434) - Add testing scenarios and update go mod
- [#6383](https://github.com/multiversx/mx-chain-go/pull/6383) - Fix some tests and add more tests

## 2. Crypto API new Opcodes [#6139](https://github.com/multiversx/mx-chain-go/pull/6139)

With this feature, new opcodes were enabled for developers, among with new crypto VM endpoints (`VerifySecp256r1`, `VerifyBLSSignatureShare`, `VerifyBLSMultiSig`).

### Impact:
* There is a new enable epoch definition for this feature, called `CryptoOpcodesV2EnableEpoch`
* There are changes to the configuration options in the gas schedule files, new values were added for `VerifySecp256r1`, `VerifyBLSSignatureShare`,`VerifyBLSMultiSig`
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

## 3. EGLD in MultiESDTTransfer [#6139](https://github.com/multiversx/mx-chain-go/pull/6139)

EGLD (native tokens) can be now sent within MultiESDTTransfer (token identifier: EGLD-000000) along with custom tokens.

Note: When using native tokens as part of MultiESDTTransfer, transaction value must be 0.

### Impact:
* EGLD transfers within MultiESDTTransfer must be handled when reading blockchain data.
* There is a new enable epoch definition for this feature, called `EGLDInMultiTransferEnableEpoch`
* No Node CLI arguments changes
* No Node HTTP API endpoints changes

### Relevant PRs:
- [#6334](https://github.com/multiversx/mx-chain-go/pull/6334) - Multi transfer execute by user flag
- [#6313](https://github.com/multiversx/mx-chain-go/pull/6313) - Added egld with multi transfer test scenario

# Smaller features or fixes

- [#5851](https://github.com/multiversx/mx-chain-go/pull/5851) - Overridable config structs
- [#6116](https://github.com/multiversx/mx-chain-go/pull/6116) - Added map support for overridable configs
- [#5897](https://github.com/multiversx/mx-chain-go/pull/5897) - Added the possibility to define more protocol IDs for p2p networks
- [#6186](https://github.com/multiversx/mx-chain-go/pull/6186) - Proposer optimisation
- [#6225](https://github.com/multiversx/mx-chain-go/pull/6225) - Update enable epochs flags
- [#6184](https://github.com/multiversx/mx-chain-go/pull/6184) - Chain simulator improvements
- [#6182](https://github.com/multiversx/mx-chain-go/pull/6182) - Chain simulator custom transactions sender component
- [#6173](https://github.com/multiversx/mx-chain-go/pull/6173) - Chain simulator force change of epoch
- [#6182](https://github.com/multiversx/mx-chain-go/pull/6182) - Chain simulator custom transactions sender component
- [#6150](https://github.com/multiversx/mx-chain-go/pull/6150) - Chain simulator updates for sovereign shards
- [#6217](https://github.com/multiversx/mx-chain-go/pull/6217) - Chain simulator tests refactor
- [#6320](https://github.com/multiversx/mx-chain-go/pull/6320) - Chain simulator extra parameter for delay between vm queries
- [#6268](https://github.com/multiversx/mx-chain-go/pull/6268) - Chain simulator verify tx on send
- [#6248](https://github.com/multiversx/mx-chain-go/pull/6248) - Chain simulator remove consensus group size
- [#6330](https://github.com/multiversx/mx-chain-go/pull/6330) - White list data verifier fixes chain simulator
- [#6018](https://github.com/multiversx/mx-chain-go/pull/6018) - TrieRecreate refactor
- [#6238](https://github.com/multiversx/mx-chain-go/pull/6238) - FIX: Destination shard id in chain simulator for meta chain addresses
- [#6292](https://github.com/multiversx/mx-chain-go/pull/6292) - Extend delegation log events for multi claim and multi redelegate rewards
- [#6117](https://github.com/multiversx/mx-chain-go/pull/6117) - Integrate new vm-common: events for "claim developer rewards"
- [#6039](https://github.com/multiversx/mx-chain-go/pull/6039) - Added missing fields on the transaction/pool by-sender request
- [#6162](https://github.com/multiversx/mx-chain-go/pull/6162) - Adjust workflow runners (MacOS)
- [#6247](https://github.com/multiversx/mx-chain-go/pull/6247) - Fixed error on withKeys
- [#6161](https://github.com/multiversx/mx-chain-go/pull/6161) - Cleanup proposal misses
- [#6322](https://github.com/multiversx/mx-chain-go/pull/6322) - Optimize "DisplayProcessTxDetails": Early exit if log level is not TRACE
- [#6333](https://github.com/multiversx/mx-chain-go/pull/6333) - Fix node/termui dockerfiles
- [#6281](https://github.com/multiversx/mx-chain-go/pull/6281) - Update Dockerfiles
- [#6275](https://github.com/multiversx/mx-chain-go/pull/6275) - Added action for building keygenerator docker images
- [#6274](https://github.com/multiversx/mx-chain-go/pull/6274) - Rename FixRelayedMoveBalanceFlag to FixRelayedBaseCostFlag + proper fix
- [#6382](https://github.com/multiversx/mx-chain-go/pull/6382) - Added fix for relayed move balance to non payables
- [#6001](https://github.com/multiversx/mx-chain-go/pull/6001) - Refactor persister factory
- [#5797](https://github.com/multiversx/mx-chain-go/pull/5797) - Use create with retries in persister factory
- [#5995](https://github.com/multiversx/mx-chain-go/pull/5995) - Persister tmp file path
- [#6010](https://github.com/multiversx/mx-chain-go/pull/6010) - Update to use latest storage version
