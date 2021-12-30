# Elrond Standard Digital Tokens (ESDT)

v1.0.0.

**[Summary](#summary)**

**[Motivation](#motivation)**

**[Fungible Tokens](#fungible-tokens)**
* [Specification](#specification)
* [Methods](#methods)
* [Issuance of fungible ESDT tokens](#issuance-of-fungible-esdt-tokens)
* [Parameters format](#parameters-format)
* [Issuance examples](#issuance-examples)
* [Transfers of ESDT tokens](#transfers-of-esdt-tokens)
* [Transfers to a Smart Contract](#transfers-to-a-smart-contract) 
* [ESDT token management](#esdt-token-management)

**[NFT and SFT Tokens](#nft-and-sft-tokens)**
* [Specification](#specification-1)
* [Methods](#methods-1)
* [Issuance of Non-Fungible Tokens (NFT)](#issuance-of-non-fungible-tokens-nft)
* [Issuance of Semi-Non-Fungible Tokens (SFT)](#issuance-of-semi-non-fungible-tokens-sft)
* [Parameters format](#parameters-format-1)
* [Issuance examples](#issuance-examples-1)
* [NFT/SFT fields](#nft-and-sft-fields)  
* [Roles](#roles)
* [Assigning roles](#assigning-roles)
* [Creation of an NFT](#creation-of-an-nft)
* [Transfer NFT Creation Role](#transfer-nft-creation-role)
* [Transfers](#transfers)
* [Example flow](#example-flow)

**[Conclusion](#conclusion)**

**[History](#history)**

## Summary
A standard interface for fungible, non-fungible (NFT) and semi-fungible (SFT) tokens that allows issuance, management and transfer of tokens, without the need for Smart Contracts (such as ERC20). ESDT tokens have native in-protocol support, so transactions with ESDT tokens do not require a VM at all. In effect, this means that ESDT tokens are as fast and as scalable as the native eGold token itself.

## Motivation
Elrond is a novel state sharded architecture. Moreover, Elrond offers a full Smart Contract development experience in such a way that Smart Contract developers do not need to know about sharding, address space, transaction broadcasting etc. 

Running Smart Contracts is done through the Elrond VM, the fastest VM in the blockchain space that is running on WASM, is extra secure, has a built-in gas model, etc. On the other hand, we acknowledge that digital tokens such as stable coins will need to have the same scalability as the base layer protocol.

With ESDT, we are proposing a novel token standard, where different digital tokens can work on top of the Elrond Network as fast as the native eGold token. This standard provides basic functionality to issue, manage, mint, burn, froze, transfer, etc. ESDT tokens.

Users also do not need to worry about sharding when transacting ESDT tokens, because the protocol employs the same handling mechanisms for ESDT transactions across shards as the mechanisms used for the eGLD native token. Sharding is therefore automatically handled and invisible to the user.
Technically, the balances of ESDT tokens held by an Account are stored directly under the data trie of that Account. It also implies that an Account can hold balances of any number of ESDT tokens, in addition to the native eGold balance. The protocol guarantees that no Account can modify the storage of ESDT tokens, neither its own nor of other Accounts.
ESDT tokens can be issued, owned and held by any Account on the Elrond network, which means that both users and Smart Contracts have the same functionality available to them. Due to the design of ESDT tokens, Smart Contracts can manage tokens with ease, and they can even react to an ESDT transfer.

## Fungible Tokens
### Specification
ESDT allows any tokens on Elrond network to be integrated by other applications and dApps: from wallets to decentralized exchanges.

### Methods
### Issuance of fungible ESDT tokens
ESDT tokens are issued via a request to the Metachain, which is a transaction submitted by the Account which will manage the tokens. When issuing a token, one must provide a token name, a ticker, the initial supply, the number of decimals for display purpose and optionally additional properties. This transaction has the form:

```
IssuanceTransaction {
    Sender: <account address of the token manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 # (0.05 EGLD)
    GasLimit: 60000000
    Data: "issue" +
          "@" + <token name in hexadecimal encoding> +
          "@" + <token ticker in hexadecimal encoding> +
          "@" + <initial supply in hexadecimal encoding> +
          "@" + <number of decimals in hexadecimal encoding>
}
```
Our initial proposal is the issuance cost to be 0.05 EGLD. Feedback and suggestions from the community is more than welcome.

Optionally, other properties can be set when issuing a token. Example:

```
IssuanceTransaction {
    Sender: <account address of the token manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 # (0.05 EGLD)
    GasLimit: 60000000
    Data: "issue" +
          "@" + <token name in hexadecimal encoding> +
          "@" + <token ticker in hexadecimal encoding> +
          "@" + <initial supply in hexadecimal encoding> +
          "@" + <number of decimals in hexadecimal encoding> +
          "@" + <"canFreeze" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canWipe" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canPause" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canMint" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canBurn" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canChangeOwner" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canUpgrade" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canAddSpecialRoles" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded>
}
```
The receiver address erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u is a built-in system smart contract (not a VM-executable contract), which only handles token issuance and other token management operations, and does not handle any transfers. The contract will add a random string to the ticker thus creating the token identifier. The random string starts with “-” and has 6 more random characters. For example, a token identifier could look like ALC-6258d2.

### Parameters format

Token Name:
* length between 3 and 20 characters
* alphanumeric characters only

Token Ticker:
* length between 3 and 10 characters
* alphanumeric UPPERCASE only

Number of decimals:
* should be a numerical value between 0 and 18
* hexadecimal encoded

Numerical values, such as initial supply or number of decimals, should be the hexadecimal encoding of the decimal numbers representing them. Additionally, they should have an even number of characters. Examples:

* 10 decimal => 0a hex encoded
* 48 decimal => 30 hex encoded
* 1000000 decimal => f4240 hex encoded

### Issuance examples
For example, a user named Alice wants to issue 4091 tokens called "AliceTokens" with the ticker "ALC". Also, the number of decimals is 6. The issuance transaction would be:

```
IssuanceTransaction {
    Sender: erd1sg4u62lzvgkeu4grnlwn7h2s92rqf8a64z48pl9c7us37ajv9u8qj9w8xg
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 # (0.05 EGLD)
    GasLimit: 60000000
    Data: "issue" +
          "@416c696365546f6b656e73" +
          "@414c43" +
          "@0ffb" +
          "@06"
}
```

Once this transaction is processed by the Metachain, Alice becomes the designated manager of AliceTokens, and is granted a balance of 4091 AliceTokens, to do with them as she pleases. She can increase the total supply of tokens at a later time if needed. For more operations available to ESDT token managers, see [Token management](#esdt-token-management).

### Transfers of ESDT tokens

Performing an ESDT transfer is done by sending a transaction directly to the desired receiver Account, but specifying some extra pieces of information in its Data field. An ESDT transfer transaction has the following form:

```
TransferTransaction {
    Sender: <account address of the sender>
    Receiver: <account address of the receiver>
    Value: 0
    GasLimit: 500000
    Data: "ESDTTransfer" +
          "@" + <token identifier in hexadecimal encoding> +
          "@" + <value to transfer in hexadecimal encoding>
}
```

While this transaction may superficially resemble a Smart Contract call, it is not. The differences are the following:

* the receiver can be any account (which may or may not be a smart contract)
* the GasLimit must be set to the value required by the protocol for ESDT transfers, namely 500000
* the Data field contains what appears to be a smart contract method invocation with arguments, but this invocation never reaches the VM: the string ESDTTransfer is reserved by the protocol and is handled as a built-in function, not as a smart contract call

Following the example from earlier, assuming that the token identifier is 414c432d363235386432, a transfer from Alice to another user, Bob, would look like this:

```
TransferTransaction {
    Sender: erd1sg4u62lzvgkeu4grnlwn7h2s92rqf8a64z48pl9c7us37ajv9u8qj9w8xg
    Receiver: erd1spyavw0956vq68xj8y4tenjpq2wd5a9p2c6j8gsz7ztyrnpxrruqzu66jx
    Value: 0
    GasLimit: 500000
    Data: "ESDTTransfer" +
          "@414c432d363235386432" +
          "@0c"
}
```

Using the transaction in the example above, Alice will transfer 12 AliceTokens to Bob.

### Transfers to a Smart Contract

The ESDT implementation can be enhanced to fix some of the flaws in the ERC-20 standard. One of the main problems with ERC-20 is that a contract does not know when someone (whether a user or another contract) sends it tokens. In Ethereum, the workaround for this, is to do 2 transactions when sending tokens to a contract:

* Call approve() on the token to allow the contract to spend tokens on your behalf.
* Call a function on the contract which then internally calls transferFrom() on the token to get the tokens you approved.


This is a waste of both gas and time and continues to be the way simply because there is already so much legacy code built on top of ERC-20. We can go even one step further to make life easier for any contract interaction and make it more general. We can add data in the txData after the transferESDT call and use that as input for the Smart Contract call. Thus we have the same way of calling a contract with eGold and with ESDT tokens.

Adding additional data effectively enables us to have the transfer and execute functionality for any Smart Contract.

Smart Contracts may hold ESDT tokens and perform any kind of transaction with them, just like any Account. However, there are a few extra ESDT features dedicated to Smart Contracts:

* **Payable versus non-payable contract:** upon deployment, a Smart Contract may be marked as payable, which means that it can receive either eGLD or ESDT tokens without calling any of its methods (i.e. a simple transfer). But by default, all contracts are non-payable, which means that simple transfers of eGLD or ESDT tokens will be rejected, unless they are method calls.

* **ESDT transfer with method invocation:** it is possible to send ESDT tokens to a contract as part of a method call, just like sending eGLD as part of a method call. A transaction that sends ESDT tokens to a contract while also calling one of its methods has the following form:

```
TransferWithCallTransaction {
    Sender: <account address of the sender>
    Receiver: <account address of the smart contract>
    Value: 0
    GasLimit: 500000 + <an appropriate amount for the method call>
    Data: "ESDTTransfer" +
          "@" + <token identifier in hexadecimal encoding> +
          "@" + <value to transfer in hexadecimal encoding> +
          "@" + <name of method to call in hexadecimal encoding> +
          "@" + <first argument of the method in hexadecimal encoding> +
          "@" + <second argument of the method in hexadecimal encoding> +
          <...>
}
```
Sending a transaction containing both an ESDT transfer and a method call allows non-payable Smart Contracts to receive tokens as part of the call, as if it were EGLD. The Smart Contract may use dedicated API functions to inspect the name of the received ESDT tokens and their amount, and react accordingly.

### ESDT token management

The Account which submitted the issuance request for an ESDT token automatically becomes the manager of the token. The manager of a token has the ability to manage the properties, the total supply and the availability of a token. Because Smart Contracts are Accounts as well, a Smart Contract can also issue and own ESDT tokens and perform management operations by sending the appropriate transactions, as shown below. One possible use case for a Smart Contract to be the issuer and/or manager of an ESDT token is if there is the need for a [Multisg Smart Contract](https://github.com/ElrondNetwork/elrond-specs/blob/main/sc-multisig-specs.md).

**Configuration properties of a fungible ESDT token**

Every ESDT token has a set of properties which control what operations are possible with it. 

The properties are:
* canMint - more units of this token can be minted by the token manager after initial issuance, increasing the supply
* canBurn - users may "burn" some of their tokens, reducing the supply
* canPause - the token manager may prevent all transactions of the token, apart from minting and burning
* canFreeze - the token manager may freeze the token balance in a specific account, preventing transfers to and from that account
* canWipe - the token manager may wipe out the tokens held by a frozen account, reducing the supply
* canChangeOwner - token management can be transferred to a different account
* canUpgrade - the token manager may change these properties
* canAddSpecialRoles - assign a specific role

## NFT and SFT Tokens

The Elrond protocol introduces native NFT support by adding metadata and attributes on top of the already existing ESDT (please read the [specifications](#specification) for fungible ESDT tokens to get familiar with the standard). This way, one can issue a semi-fungible token or a non-fungible token which is quite similar to an ESDT, but has a few more attributes, such as a changeable URI. 

Once owning a quantity of a NFT/SFT, users will have their data store directly under their account, inside the trie. All the fields available inside a NFT/SFT token can be found [here](#nft-and-sft-fields).

The flow of issuing and transferring non-fungible or semi-fungible tokens is:

* register/issue the token
* set roles to the address that will create the NFT/SFTs
* create the NFT/SFT
* transfer quantity(es)

### Specification
ESDT allows any tokens on Elrond network to be integrated by other applications and dApps: from wallets to decentralized exchanges.

### Methods

### Issuance of Non-Fungible Tokens (NFT)

One has to perform an issuance transaction in order to register the token. Non-Fungible Tokens are issued via a request to the Metachain, which is a transaction submitted by the Account which will manage the tokens. When issuing a token, one must provide a token name, a ticker and optionally additional properties. This transaction is very similar to the issue of a fungible ESDT token and has the form:

```
IssuanceTransaction {
    Sender: <account address of the token manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 (0.05 EGLD)
    GasLimit: 60000000
    Data: "issueNonFungible" +
          "@" + <token name in hexadecimal encoding> +
          "@" + <token ticker in hexadecimal encoding>
}
```
Optionally, the properties can be set when issuing a token. Example:

```
IssuanceTransaction {
    Sender: <account address of the token manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 (0.05 EGLD)
    GasLimit: 60000000
    Data: "issueNonFungible" +
          "@" + <token name in hexadecimal encoding> +
          "@" + <token ticker in hexadecimal encoding> +
          "@" + <"canFreeze" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canWipe" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canPause" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canTransferNFTCreateRole" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded>
}
```

The receiver address erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u is a built-in system smart contract (not a VM-executable contract), which only handles token issuance and other token management operations, and does not handle any transfers. The contract will add a random string to the ticker thus creating the token identifier. The random string starts with “-” and has 6 more random characters. For example, a token identifier could look like ALC-6258d2.

### Issuance of Semi-Non-Fungible Tokens (SFT)

One has to perform an issuance transaction in order to register the token. Semi Fungible Tokens are issued via a request to the Metachain, which is a transaction submitted by the Account which will manage the tokens. When issuing a semi fungible token, one must provide a token name, a ticker, the initial quantity and optionally additional properties. This transaction has the form:

```
IssuanceTransaction {
    Sender: <account address of the token manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 (0.05 EGLD)
    GasLimit: 60000000
    Data: "issueSemiFungible" +
          "@" + <token name in hexadecimal encoding> +
          "@" + <token ticker in hexadecimal encoding>
}
```

Optionally, the properties can be set when issuing a token. Example:

```
IssuanceTransaction {
    Sender: <account address of the token manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 (0.05 EGLD)
    GasLimit: 60000000
    Data: "issueSemiFungible" +
          "@" + <token name in hexadecimal encoding> +
          "@" + <token ticker in hexadecimal encoding> +
          "@" + <"canFreeze" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canWipe" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canPause" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded> +
          "@" + <"canTransferNFTCreateRole" hexadecimal encoded> + "@" + <"true" or "false" hexadecimal encoded>
}
```

The receiver address erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u is a built-in system smart contract (not a VM-executable contract), which only handles token issuance and other token management operations, and does not handle any transfers. The contract will add a random string to the ticker thus creating the token identifier. The random string starts with “-” and has 6 more random characters. For example, a token identifier could look like ALC-6258d2.

### Parameters format

Token Name:
* length between 3 and 20 characters
* alphanumeric characters only

Token Ticker:
* length between 3 and 10 characters
* alphanumeric UPPERCASE only

### Issuance examples
For example, a user named Alice wants to issue an ESDT called "AliceTokens" with the ticker "ALC". The issuance transaction would be:
```
IssuanceTransaction {
    Sender: erd1sg4u62lzvgkeu4grnlwn7h2s92rqf8a64z48pl9c7us37ajv9u8qj9w8xg
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 5000000000000000000
    GasLimit: 60000000
    Data: "issueSemiFungible" +
          "@416c696365546f6b656e73" +
          "@414c43" +
}
```
Once this transaction is processed by the Metachain, Alice becomes the designated manager of AliceTokens. In case of NFTs and SFTs Alice does not need to mint any tokens, but she will need to register a ticker so that she receives the tokenID. She can create and add quantity later using ESDTNFTCreate. For more operations available to ESDT token managers, see [ESDT-Token management](#esdt-token-management)

### Roles
In order to be able to perform actions over a token, one needs to have roles assigned. The existing roles are:

For ESDT:
* ESDTRoleLocalMint: this role allows minting new tokens at the account
* ESDTRoleLocalBurn: this role allows burning tokens at the account

For NFT:
* ESDTRoleNFTCreate : this role allows one to create a new NFT
* ESDTRoleNFTBurn : this role allows one to burn quantity of a specific NFT

For SFT:
* ESDTRoleNFTCreate : this role allows one to create a new SFT
* ESDTRoleNFTBurn : this role allows one to burn quantity of a specific SFT
* ESDTRoleNFTAddQuantity : this role allows one to add quantity of a specific SFT

### Assigning roles

Roles can be assigned by sending a transaction to the Metachain from the ESDT manager.
Within a transaction of this kind, any number of roles can be assigned (minimum 1).

```
RolesAssigningTransaction {
    Sender: <address of the ESDT manager>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 0
    GasLimit: 60000000
    Data: "setSpecialRole" +
          "@" + <token identifier in hexadecimal encoding> +
          "@" + <address to assign the role(s) in a hexadecimal encoding> +
          "@" + <role in hexadecimal encoding> +
          "@" + <role in hexadecimal encoding> +
          ...
}
```

For example, ESDTRoleNFTCreate = 45534454526f6c654e4654437265617465

### NFT and SFT fields

Below you can find the fields involved when creating an NFT.

**NFT Name**
- The name of the NFT or SFT

**Quantity**
- The quantity of the token. If NFT, it must be `1`

**Royalties**
- Allows the creator to receive royalties for any transaction involving their NFT
- Base format is a numeric value between 0 an 10000 (0 meaning 0% and 10000 meaning 100%)

**Hash**
- Arbitrary field that should contain the hash of the NFT metadata.

**Attributes**
- Arbitrary field that should contain a set of attributes in the format desired by the creator

**URI(s)**
- Minimum one field that should contain the `Uniform Resource Identifier`. Can be a URL to a media file or something similar.

Note that each argument must be encoded in hexadecimal format with an even number of characters.

### Creation of an NFT

A single address can own the role of creating an NFT for an ESDT token. This role can be transferred by using the ESDTNFTCreateRoleTransfer function.

A NFT can be created on top of an existing ESDT by sending a transaction to self that contains the function call that triggers the creation. Any number of URIs can be assigned (minimum 1):
```
NFTCreationTransaction {
    Sender: <address with ESDTRoleNFTCreate role>
    Receiver: <same as sender>
    Value: 0
    GasLimit: 6000000 + Additional gas (see below)
    Data: "ESDTNFTCreate" +
          "@" + <token identifier in hexadecimal encoding> +
          "@" + <initial quantity in hexadecimal encoding> +
          "@" + <NFT name in hexadecimal encoding> +
          "@" + <Royalties in hexadecimal encoding> +
          "@" + <Hash in hexadecimal encoding> +
          "@" + <Attributes in hexadecimal encoding> +
          "@" + <URI in hexadecimal encoding> +
          "@" + <URI in hexadecimal encoding> +
          ...
}
```

Additional gas refers to:
- Transaction payload cost: Data field length * 1500 (GasPerDataByte = 1500)
- Storage cost: Size of NFT data * 50000 (StorePerByte = 50000)

Note that because NFTs are stored in accounts trie, every transaction involving the NFT will require a gas limit depending on NFT data size.

### Transfer NFT Creation Role

This role can be transferred only if the canTransferNFTCreateRole property of the token is set to true.

The role of creating an NFT can be transferred by a Transaction like this:
```
TransferCreationRoleTransaction {
    Sender: <address of the current creation role owner>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 0
    GasLimit: 60000000 + length of Data field
    Data: "ESDTNFTCreateRoleTransfer" +
          "@" + <token identifier in hexadecimal encoding> +
          "@" + <the address to transfer the role from in hexadecimal encoding> + 
          "@" + <the address to transfer the role to in hexadecimal encoding> 
}
```

### Transfers

Performing an ESDT NFT or SFT transfer is done by specifying the receiver's address inside the Data field, alongside other details. An ESDT NFT/SFT transfer transaction has the following form:
```
TransferTransaction {
    Sender: <account address of the sender>
    Receiver: <same as sender>
    Value: 0
    GasLimit: 1000000 + length of Data field
    Data: "ESDTNFTTransfer" +
          "@" + <token identifier in hexadecimal encoding> +
          "@" + <the nonce after the NFT creation in hexadecimal encoding> + 
          "@" + <quantity to transfer in hexadecimal encoding> +
          "@" + <destination address in hexadecimal encoding>
}
```

### Example flow

Let's see a complete flow of creating and transferring a Semi Fungible Token.


**Step 1:** Issue/Register a Semi Fungible Token

```
{
    Sender: <your address>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 50000000000000000 # (0.05 EGLD)
    GasLimit: 60000000
    Data: "issueSemiFungible" +
          "@416c696365546f6b656e73" + # AliceTokens
          "@414c43" +                 # ALC
}
```

**Step 2:** Fetch the token identifier

For doing this, one has to check the previously sent transaction and see the Smart Contract Result of it. It will look similar to @ok@414c432d317132773365. The 414c432d317132773365 represents the token identifier in hexadecimal encoding.

**Step 3:** Set roles

Assign ESDTRoleNFTCreate and ESDTRoleNFTAddQuantity roles to an address. You can set these roles to your very own address.
```
{
    Sender: <your address>
    Receiver: erd1qqqqqqqqqqqqqqqpqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzllls8a5w6u
    Value: 0
    GasLimit: 60000000
    Data: "setSpecialRole" +
          "@414c432d317132773365" +   # previously fetched token identifier
          "@" + <address to assign the roles in a hexadecimal encoding> +
          "@45534454526f6c654e4654437265617465" +  # ESDTRoleNFTCreate
          "@45534454526f6c654e46544164645175616e74697479" # ESDTRoleNFTAddQuantity
          ...
}
```

**Step 4:** Create NFT

Fill all the attributes as you think.
```
{
    Sender: <address with ESDTRoleNFTCreate role>
    Receiver: <same as sender>
    Value: 0
    GasLimit: 60000000
    Data: "ESDTNFTCreate" +
          "@414c432d317132773365" +   # previously fetched token identifier
          "@" + <initial quantity in hexadecimal encoding> +
          "@" + <NFT name in hexadecimal encoding> +
          "@" + <Royalties in hexadecimal encoding> +
          "@" + <Hash in hexadecimal encoding> +
          "@" + <Attributes in hexadecimal encoding> +
          "@" + <URI in hexadecimal encoding> +
          "@" + <URI in hexadecimal encoding> +
}
```

Note that the nonce is very important when creating an NFT. You must save the nonce after NFT creation because you will need it for further actions. For example, if Alice has nonce 5 before sending the NFT creation transaction, nonce 6 must be used in further actions. Also, the nonce can be fetched by viewing all the tokens for the address via API.

**Step 5:** Transfer
```
{
    Sender: <your address>
    Receiver: <same as sender>
    Value: 0
    GasLimit: 1000000 + length of Data field
    Data: "ESDTNFTTransfer" +
          "@414c432d317132773365" +   # previously fetched token identifier
          "@" + <the nonce saved above in hexadecimal encoding> + 
          "@" + <quantity to transfer in hexadecimal encoding> +
          "@" + <destination address in hexadecimal encoding>
}
```

## Conclusion

Multiple custom tokens, having the same scalability and speed as the native token, in a sharded open-blockchain, without the need for Smart Contracts, is what ESDT is all about.

## History

See also these discussions:
* https://docs.elrond.com/developers/esdt-tokens/ 
* https://docs.elrond.com/developers/nft-tokens/ 
