## Transaction

```
dto Transaction:
    sender: string;
    receiver: string;
    gasLimit: uint32;
    chainID: string;

    nonce?: uint64;
    value?: (string|bigNumber);
    senderUsername?: string;
    receiverUsername?: string;
    gasPrice?: uint32;

    data?: bytes;
    version?: uint32;
    options?: uint32;
    guardian?: string;

    signature: bytes;
    guardianSignature?: bytes;
```

## TransactionComputer

```
class TransactionComputer:
    compute_transaction_fee(transaction: Transaction, network_config: INetworkConfig): bigNumber;
    compute_bytes_for_signing(transaction: Transaction): bytes;
    compute_transaction_hash(transaction: Transaction): bytes;
```
