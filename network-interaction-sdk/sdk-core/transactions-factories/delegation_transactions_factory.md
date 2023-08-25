## DelegationTransactionsFactory

```
class DelegationTransactionIntentsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", gas limit for specific operations etc. (e.g. "gasLimitForStaking").

    create_transaction_intent_for_new_delegation_contract({
        sender: IAddress;
        totalDelegationCap: (string|bigNumber);
        service_fee: number;
        value: (string|bigNumber);
    }): TransactionIntent;

    create_transaction_intent_for_adding_nodes({
        sender: IAddress;
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
        signedMessages: List[bytes];
    }): TransactionIntent;

    create_transaction_intent_for_removing_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): TransactionIntent;

    create_transaction_intent_for_staking_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): TransactionIntent;

    create_transaction_intent_for_unbonding_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): TransactionIntent;

    create_transaction_intent_for_unstaking_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): TransactionIntent;

    create_transaction_intent_for_unjailing_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): TransactionIntent;

    create_transaction_intent_for_changing_service_fee({
        sender: IAddress;
        delegationContract: IAddress;
        serviceFee: int;
    }): TransactionIntent;

    create_transaction_intent_for_modifying_delegation_cap({
        sender: IAddress;
        delegationContract: IAddress;
        delegationCap: int;
    }): TransactionIntent;

    create_transaction_intent_for_setting_automatic_activation({
        sender: IAddress;
        delegationContract: IAddress;
    }): TransactionIntent;

    create_transaction_intent_for_unsetting_automatic_activation({
        sender: IAddress;
        delegationContract: IAddress;
    }): TransactionIntent;

    create_transaction_intent_for_setting_cap_check_on_redelegate_rewards({
        sender: IAddress;
        delegationContract: IAddress;
    }): TransactionIntent;

    create_transaction_intent_for_unsetting_cap_check_on_redelegate_rewards({
        sender: IAddress;
        delegationContract: IAddress;
    }): TransactionIntent;

    create_transaction_intent_for_setting_metadata({
        sender: IAddress;
        delegationContract: IAddress;
        name: str;
        website: str;
        identifier: str;
    }): TransactionIntent;

    ...
```
