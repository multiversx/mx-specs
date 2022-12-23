## Name of the project
Elrond Buddies

## Description of the project
Elrond Buddies is a NFT project composed of 10,000 unique crypto collectibles on MultiversX.

website: https://www.elrondbuddies.com/

**Old PR**: https://github.com/ElrondNetwork/elrond-specs/pull/19

## Purpose
We send to our holders monthly rewards with LKMEX, UTK, RIDE, ITHEUM and WATER.

To farm LKMEX on the dex we use this following wallet called 'ebuddiesdao', every month 20% of what we generated is reinvested in the LKMEX farm and we distribute 80% to the holders.
https://explorer.elrond.com/accounts/erd16n5yll0yver02j4wm0l5fzw79c0dqg8ra3exq3ag35n7tfuwwxksxxzwzt

We started distributing LKMEX on the 21/04/2022, so far we have distributed 73,002,098 LKMEX to our holders and reinvested 14,756,856 LKMEX

We used two python scripts that we developed to get the holders and to distribute them the LKMEX:

https://github.com/xdevguild/multiversX-nft-holders
https://github.com/xdevguild/esdt-and-lkmek-airdrop-scripts

Now that you guys want us to use a SC to distribute LKMEX we found a new way to do it with it.

## Address of Smart Contract (Devnet)
https://devnet-explorer.elrond.com/accounts/erd1qqqqqqqqqqqqqpgqwk5lgpf2fdf2veej683ltr2gzcyr0al5y7zswazjc5

When you reviewed this SC we will deploy it on Mainnet. 

## Smart Contract Code
https://github.com/xdevguild/sc-multi-sender-rs

## Description for every Smart Contract functionality
**multiSendNft:**  

    The Multi Send Nft function only accepts NFT. It receives the following arguments:
    The NFT Collection Identifier you want to send
    One address you want to send to
    The nonce OF the NFT you want to send to the address

## Number of users
On the last distribution (21/11/2022) we sent LKMEX to 371 wallets (holders)

## Average LKMEX used during the previous month
On the last distribution (21/11/2022) we sent around 16,000,000 LKMEX

Thank you for the work, do not hesitate to ask questions :)
