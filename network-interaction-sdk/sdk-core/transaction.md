## Transaction

```
dto Transaction:
    sender: string;
    receiver: string;
    gasLimit: uint32;
    chainID: string;

    nonce?: uint64;
    value?: Amount;
    senderUsername?: string;
    receiverUsername?: string;
    gasPrice?: uint32;

    data?: bytes;
    version?: uint32;
    options?: uint32;
    guardian?: string;

    signature: bytes;
    guardianSignature?: bytes;

    // Optional named constructor, if and only if the implementing library defines a `DraftTransaction`.
    new_from_draft(draft: DraftTransaction): Transaction;
```

## DraftTransaction

Optionally, if desired, the implementing library can also define an incomplete representation of the transaction, to be used as return type for the **transaction factories**. See [README](../README.md), instead of the `Transaction` type.

```
dto DraftTransaction:
    sender: string;
    receiver: string;
    value?: string;
    data?: bytes;
    gasLimit: uint32;
```

## TransactionComputer

```
class TransactionComputer:
    compute_transaction_fee(transaction: Transaction, network_config: INetworkConfig): Amount;
    compute_bytes_for_signing(transaction: Transaction): bytes;
    compute_transaction_hash(transaction: Transaction): bytes;
```
