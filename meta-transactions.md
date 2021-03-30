# Meta-transactions Standard (MTS)

## Summary

All transactions on the Elrond blockchain must have sufficient gas in order to be processed, gas that is paid in eGold, the native token of the Elrond blockchain. Meta-transactions are probably best summarized through the effect they have: users can interact with the Elrond blockchain without holding and paying any eGold. 

## Abstract

Forcing users to buy-and-hold eGold in order to register as an user of a dApp on Elrond blockchain (like Maiar for example) would be a terrible experience for just about everyone, especially for non-crypto users. This is the problem that meta-transactions attempt to solve.

The internet has created a "free-to-use" culture for most of its popular apps. If their decentralized counterparts are to replace them and gain traction, they should avoid raising a paywall, no matter how inexpensive, in front of its users.

Meta-transactions, also referred to as gasless transactions or relayed transactions, is the common name for those transactions which consume no gas as eGold from the user’s perspective. However, all transactions on the Elrond blockchain must have sufficient gas - in order to protect the validators and avoid malicious DOS attacks - therefore it is required to add support for transactions that allow the usage of dApps without owning and paying eGold as gas, but which take the gas from a different source.

Because gas still has to be paid for any transaction, we introduce the concept of relayer, which is the entity that pays for the gas on behalf of the users of a dApp. A meta-transaction process includes several steps: 

* the user (and its associated address) signs a transaction and sends it to a relayer. Each user’s transaction carries a nonce. 
* this transaction is exactly like a normal transaction but instead of being send to the blockchain it is sent off-chain to the relayer
* it is up to each relayer to put in place its own policy and strategy in regards with what transactions are accepted, from what source, frequency, anti-spam etc.
* the relayer wrapps the signed transaction from the user into its own transaction by constructing and signing a meta-transaction; 
* lastly, the Elrond protocol verifies if the inner user’s transaction is indeed signed by the user, is correctly constructed and whether it has the correct nonce. If these verifications pass, the meta-transaction is executed.

On the user side, sending a meta-transaction is similar to sending a standard transaction (from, to, value ... and signature) except that instead of sending it directly to the Elrond blockchain, it sends the meta-transaction to a third party (the relayer) who will take care of the gas.

One of the first dApps which needs this kind of transactions is Maiar: the official Elrond mobile wallet app. When a user registers an account on Maiar, a new or already existing Elrond address: erd1… is mapped on the Elrond blockchain to the user’s mobile phone number. The mapping process involves several transactions on Elrond blockchain, meta-transactions that from the user perspective are done by Maiar and incur for the user zero fees.

On Maiar, a user can also choose to create a friendly username: @herotag, that is associated with his eGold (erd1…) address. The @herotag is registered on Elrond blockchain and the registration can be done through meta-transactions. This service is known as DNS service on the Elrond blockchain.

Furthermore, the Elrond Standard Digital Token (ESDT) is in dire need of this functionality as well. Some users might hold only stablecoins, without any eGold to pay for the gas. Meta-transactions are thus of utmost importance for these users. Some of these users may want to pay the gas but not in eGold but in the token they hold (it depends on the relayer if it accepts payment in that token) or some relayers may want to make all the transactions free for their users (the relayer would pay the gas fee).  For example, a relayer may offer a yield farming service on Elrond network, and may be incentivised to allow its users to quickly offer liquidity to its pools even if they do not own any eGold to pay for the actual deposit transactions.

The above mentioned use cases are just a few examples where meta-transactions are needed by removing the friction for users. It is up to developers to integrate this type of transactions and experiment the best onboarding experiences and different other approaches.

## Flow example 
* Alice wants to send some stablecoins directly to Bob: $10 as USDt. They both have accounts on Elrond blockchain but Alice does not have eGold. She has only USDT in the form of stablecoins.

* Alice opens the wallet and signs a message that she wants to send $10 to Bob, but without any fee, because the fee will be supported by a relayer.

* The message signed by Alice must now be wrapped into a real transaction. The wallet creates a real transaction where the SENDER is the GASLESS TX relayer (this account has eGold), and the receiver is the ADDRESS of Alice (the relayer sends to ALICE the gas and from that the inner transaction of Alice those 10$ goes to Bob). This way, the transaction contains Alice’s signed message, describing the transfer of $10 to Bob, but the gas for this transaction will be paid by the relayer.

* The relayer signs the final constructed transaction and pays the transaction fee.

* The protocol verifies if the inner message is indeed signed by Alice, is correctly constructed and whether it has the correct nonce. If these verifications pass, the transfer is executed.

The message that Alice has to sign must be clear, easy to understand and should have all the functionalities of a normal transaction.

Note: with meta-transactions, users never relinquish control of their private keys. The worst a relayer can do is refuse to send Alice’s transaction. If so, Alice can use a different relayer.

## High level requirements

* Easy to integrate with wallets
* Standard way of validation and creation
* The wrapper transaction must contain all the information a smart contract would need
* Protect the user from replay attacks

## Terminology and roles

**User** - it is the one who wants to transfer ESDT or to call a smart contract without paying fees in eGold

**Relayer** - the one who creates the transaction with the data given by the user. Fees are paid by the relayer.

**Smart contract** - it is called by the user through the meta-transaction (could be owned by the relayer, but not necessarily).

**ESDT token** - a token stored on Elrond’s extended data structure of an account, in parallel to the usual eGold balance; useful in many cases, but especially when a user wants to transfer other tokens without paying gas in eGold. 

## Specification overview

In Ethereum, any contract which supports gasless transactions must do all the work itself. It has to verify if the Data field of the wrapper transaction has the correct signed message by the actual sender, whether that transaction was correctly constructed and they should also add protection against replay attacks (the relayer sending the same signed message by Alice multiple times and executing it only once). By implementing this processing and verification at the Smart Contract level, it is harder to develop these smart contracts, relying on a lot of boilerplate code, and allowing for more places where mistakes can occur. Furthermore it costs more to execute in the VM, because every processing step has to be paid.

Elrond comes with a new innovation by supporting meta transactions directly at the protocol level, verifying the transaction before the smart contract / built-in function execution in order to protect the users. This makes it easier for Smart Contract developers to enable this functionality (no need to develop it), it costs less as it is executed at native speed - not in VM. 

## Specification

Because the message from the user is effectively a transaction, we will use a marshallized transaction as the transaction DATA.

So the first steps of processing a meta-transaction would be:
* decode the underlying inner transaction (from the tx.Data) 
* to verify the user’s signature
* verifies the correctness of it (nonce check, gas check, value check) 
* take out the gas and value from the relayer balance and continue with the processing of the underlying transaction that is signed by the user (by Alice in our example above).

In order to make it even more visible to the user what exactly will be signed and to make it easier to be be integrated in any wallet, we can set the DATA field of the transaction to the following: relayedTX@hex.encode(marshalizedTXSignedByUser)

This means that the transaction is of type meta-transaction, whose actual beneficiary is the user (Alice)  and the actual transaction is under the marshalizedTX which is signed by the user.

This means that even at interceptor level we can easily verify the validity of the transaction by checking the signatures. The marshalized transaction signed by the user is actually a normal transaction with all the fields, whose validity can be verified the same way as any other transaction.

The user, when interacting with the relayer and the wallet, will first see her full transaction, and after that, she can sign it and can check the resulting data whether it is correct. The user might need to specify that X% amount of Y token will be paid to the relayer - although this is not mandatory, as that depends on the relayer whether the transaction is completely free or the user should pay in some way. If the relayer accepts an ESDT token as compensation for the gas paid in eGold, a second meta-transaction can be created with that ESDT fee where the receiver is the relayer itself and the amount is a fixed value or a specific % of the amount sent (up to the relayer to set up this and inform the user through the wallet or dApp).

After signing this transaction, the user sends the signed transaction, through the wallet, to the relayer (off-chain). Finally the relayer creates the final transaction where the SENDER is the relayer's address, while the RECEIVER will be the user.

## Protocol and Smart Contracts interactions

The meta-transaction will start its journey towards execution in the shard where the relayer is located (relayer systems probably will use at least one relayer account in each shard, to avoid cross-shard latencies). The meta-transaction is verified by the interceptors (validators’ client software basically) at first: after seeing that the transaction is of type relayedTX, the interceptor can easily verify the validity and integrity of the inner transaction (marshalized and signed by the user). If valid, it will be added to the data pool. Upon processing the transaction, the whole transaction fee will be taken out of the relayer's balance. The inner transaction is decoded and this transaction (the one signed by the user) will be executed according to the information there. First we translate the inner transaction into a smart contract result and then it continues execution of the inner transaction, which starts from Alice.

There is an extra verification for the amount of gas which must keep the next invariant:
relayedTx.GasLimit == innerTx.GasLimit + computeGasCostOf(relayedTx)
computeGasCostOf(relayedTx) = 50000 + DataCopyPerByte * len(relayedTx.Data)

If the relayer is in another shard then the user, the processing in the current shard stops here and the meta-transaction will go to the destination shard as the normal cross shard processing goes. When the transaction reaches the shard where the user is, it will verify if the nonce of the inner transaction is correct (must keep invariant of innerTx.nonce == userAccount.nonce) as this is the replay attack protection (the same inner transaction cannot be processed 2 times). The protocol will translate the inner transaction into a smart contract result whose SENDER is the user and will continue the processing of that as usual. 

After finishing the processing of the created smart contract result, the unused gas - the final gasRemaining - will be sent back to the relayer.

## Conclusion
Meta-transactions are a method of allowing individuals to interact with the Elrond blockchain without holding any eGold and without ever relinquishing control of their private keys. This allows dApps to dramatically improve the users’ experiences.

It is necessary to have eGold on your account if you want to change a smart contract or send a transaction or do anything on Elrond. This makes user onboarding and user experience painful. There is no way to have a high conversion rate when the users have to buy eGold before being able to use an application.      

To overcome that, some applications might simply offer to pay gas on behalf of their users. This approach is often disregarded by developers as it is far from the philosophy of the protocol, which aims to guarantee independence in the network.

Since income generation is generally correlated to the use of smart contracts, it is an economically viable model in most cases. As Elrond has smart contract royalties (30% of the consumed gas goes to the developer) we can easily see that from these royalties, the smart contract developer could finance the transactions fees of its (new) users when they are onboarding. The developer may choose to support the transaction costs for a set of users, or,  when the developer has other sources of revenue, they could simply pay for all the transactions. In this case, the developer is the relayer.   

The application and the users might also agree with each other on-chain for an automatic reimbursement of gas costs implied. With the emergence of DeFi's services, more and more users might own ESTD tokens before owning any eGold. And some dApps might allow their users to pay for their transactions with an ESDT token. The application can also distribute its own tokens to its users at the time of registration. A relayer will help to put in place the right gas reimbursement strategy without requiring heavy investments in research and development.
