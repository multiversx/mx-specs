## TransferTransactionsFactory

A class that provides methods for creating transactions for transfers (native or ESDT).

```
class TransferTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", "issueCost", gas limit for specific operations etc. (e.g. "gasLimitForESDTTransfer").
    
    create_transaction_for_native_token_transfer({
        sender: IAddress;
        receiver: IAddress;
        native_amount: Amount;
    }): Transaction;

    // Can throw:
    // - ErrBadArguments
    //
    // If multiple transfers are specified, a multi-ESDT transfer transaction should be created.
    // Bad usage should be reported.
    create_transaction_for_esdt_token_transfer({
        sender: IAddress;
        receiver: IAddress;
        token_transfers: TokenTransfer[];
    }): Transaction;
```
