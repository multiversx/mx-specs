## TransferIntentsFactory

A class that provides methods for creating transactions for transfers (native or ESDT).

```
class TransferIntentsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", "issueCost", gas limit for specific operations etc. (e.g. "gasLimitForESDTTransfer").
    
    create_transaction_intent_for_native_transfer({
        sender: IAddress;
        receiver: IAddress;
        native_transfer_amount: Amount;
    }): TransactionIntent;

    // Can throw:
    // - ErrBadArguments
    //
    // All kinds of transfers (native and ESDT) should be handled.
    // If multiple transfers are specified, a multi-ESDT transfer intent should be created.
    // Bad usage should be reported.
    create_transaction_intent_for_custom_transfers({
        sender: IAddress;
        receiver: IAddress;
        custom_transfers: CustomTokenTransfer[];
    }): TransactionIntent;
```
