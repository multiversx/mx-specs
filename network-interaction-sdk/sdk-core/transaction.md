## Transaction

```
class Transaction:
    constructor({
        sender: IAddress;
        receiver: IAddress;
        gasLimit: IGasLimit;
        chainID: IChainID;

        nonce?: INonce;
        value?: ITransactionValue;
        senderUsername?: string;
        receiverUsername?: string;
        gasPrice?: IGasPrice;

        data?: ITransactionPayload;
        version?: ITransactionVersion;
        options?: ITransactionOptions;
        guardian?: IAddress;
    });

    // Getters and setters for all the fields.
    // ...

    // Optional: alias for setSignature().
    applySignature(signature: bytes): void;
    // Optional: alias for setGuardianSignature().
    applyGuardianSignature(signature: bytes): void;

    serializeForSigning(): bytes;

    // Returns the transaction as a plain object.
    toPlainObject(withSignatures: bool = true): any;

    // Optional: alias for `toPlainObject()`.
    toJSON(): any; // JavaScript
    to_dictionary(): any; // Python
```

Module-level functions:

```
computeFee(transaction: ITransaction, networkConfig: INetworkConfig): bigNumber;
computeTransactionHash(transaction: ITransaction): bytes;
```
