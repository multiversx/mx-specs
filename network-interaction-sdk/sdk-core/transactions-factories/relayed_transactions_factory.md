## RelayedTransactionsFactory

class RelayedTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", gas limit for specific operations etc.

    create_relayed_v1_transaction({
        inner_transaction: ITransaction;
        relayer_address: IAddress;
        relayer_nonce: Optional[int];
    }): Transaction;

    // can throw InvalidGasLimitForInnerTransaction
    create_relayed_v2_transaction({
        inner_transaction: ITransaction;
        relayer_address: IAddress;
        relayer_nonce: Optional[int];
    });
