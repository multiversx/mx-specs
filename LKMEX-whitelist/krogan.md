## Name of the project
Krogan Marketplace

## Description of the project
NFT Marketplace with unique features like NFT swapping, NFT lending, shared revenue, etc. But one important feature for which we're making this request is the "IN-WALLET STAKING". A feature that helps nft projects deliver rewards to their holders without forcing their users to move their nft inside of a contract.

## Purpose
Our marketplace elrondnftswap.com (rebranding coming soon) helps various projects to reward their holders using a token of their choice. Some projects picked LKMEX as being that reward and they would like to continue to do so. 
Based on the rules, this is an acceptable behaviour as it's the classic reward system that a lot of NFT projects is using.
An example of usage in production: https://elrondnftswap.com/collection/EGLDRUSH-90a737 scroll down to "Weekly rewards" section.

## Address of Smart Contract
https://explorer.elrond.com/accounts/erd1qqqqqqqqqqqqqpgqkg84sqdc3za05ce3r4nqqdpeqzx429k4we0s8wp0u2

The contract has already 4175 transactions and large deposits of LKMEX.

## Smart Contract Code
https://github.com/dan-merlea/sc-krogan-public/tree/main/nft-airdrop

## Description for every Smart Contract functionality
Metabonding similar contract to send LKMEX along with other types of rewards to holders of NFTs.
Whitelisted addresses / project owners (whitelistAddress) can deposit a token of their choice (addRewardsCheckpoint) every week after the api makes a snapshot of the NFT collection. Users can then claim the rewards based on the amount of NFTs they hold during the snapshot times (claimRewards). The claim is using a signature based on a hash that makes it impossible to claim the same rewards twice.

## Number of users
2000+

## Average LKMEX used during the previous month
~70,000,000 LKMEX