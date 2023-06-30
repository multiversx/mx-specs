## DelegationTransactionsFactory

```
class DelegationTransactionsFactory:
    // IConfig should be a private (internal) interface that defines the necessary configuration (e.g. minGasLimit, gasLimitPerByte, issueCost).
    constructor(config: IConfig);

    create_transaction_for_new_delegation_contract

    create_transaction_for_adding_nodes

    create_transaction_for_removing_nodes

    create_transaction_for_staking_nodes

    create_transaction_for_unbonding_nodes

    create_transaction_for_unstaking_nodes

    create_transaction_for_unjailing_nodes

    create_transaction_for_changing_service_fee

    create_transaction_for_modifying_delegation_cap

    create_transaction_for_setting_automatic_activation

    create_transaction_for_unsetting_automatic_activation

    create_transaction_for_setting_redelegate_cap

    create_transaction_for_unsetting_redelegate_cap

    create_transaction_for_setting_metadata
```
