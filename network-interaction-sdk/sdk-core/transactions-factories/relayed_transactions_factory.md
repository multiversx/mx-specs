## RelayedTransactionsFactory

class RelayedTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", gas limit for specific operations etc.

    // Each implementation is responsible to check that each inner transaction is signed.
    // Can throw InvalidInnerTransactionError

    create_relayed_v1_transaction({
        inner_transaction: ITransaction;
        relayer_address: IAddress;
    }): Transaction;

    // can throw InvalidInnerTransactionError if inner_transaction.gas_limit != 0
    create_relayed_v2_transaction({
        inner_transaction: ITransaction;
        inner_transaction_gas_limit: uint32;
        relayer_address: IAddress;
    });
