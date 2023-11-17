## DelegationTransactionsFactory

```
class DelegationTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", gas limit for specific operations etc. (e.g. "gasLimitForStaking").

    create_transaction_for_new_delegation_contract({
        sender: IAddress;
        totalDelegationCap: Amount;
        service_fee: number;
        amount: Amount;
    }): Transaction;

    create_transaction_for_adding_nodes({
        sender: IAddress;
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
        signedMessages: List[bytes];
    }): Transaction;

    create_transaction_for_removing_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_staking_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_unbonding_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_unstaking_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_unjailing_nodes({
        sender: IAddress,
        delegationContract: IAddress;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_changing_service_fee({
        sender: IAddress;
        delegationContract: IAddress;
        serviceFee: int;
    }): Transaction;

    create_transaction_for_modifying_delegation_cap({
        sender: IAddress;
        delegationContract: IAddress;
        delegationCap: int;
    }): Transaction;

    create_transaction_for_setting_automatic_activation({
        sender: IAddress;
        delegationContract: IAddress;
    }): Transaction;

    create_transaction_for_unsetting_automatic_activation({
        sender: IAddress;
        delegationContract: IAddress;
    }): Transaction;

    create_transaction_for_setting_cap_check_on_redelegate_rewards({
        sender: IAddress;
        delegationContract: IAddress;
    }): Transaction;

    create_transaction_for_unsetting_cap_check_on_redelegate_rewards({
        sender: IAddress;
        delegationContract: IAddress;
    }): Transaction;

    create_transaction_for_setting_metadata({
        sender: IAddress;
        delegationContract: IAddress;
        name: str;
        website: str;
        identifier: str;
    }): Transaction;

    create_transaction_for_delegating({
        sender: IAddress;
        delegationContract: IAddress;
        value: Amount;
    }): Transaction;

    create_transaction_for_claiming_rewards({
        sender: IAddress;
        delegationContract: IAddress;
    }): Transaction;

    create_transaction_for_redelegating_rewards({
        sender: IAddress;
        delegationContract: IAddress;
    }): Transaction;

    create_transaction_for_undelegating({
        sender: IAddress;
        delegationContract: IAddres;
        amount_to_undelegate: Amount;
    }): Transaction;

    create_transaction_for_withdrawing({
        sender: IAddress;
        delegationContract: IAddress;
    })

    ...
```
