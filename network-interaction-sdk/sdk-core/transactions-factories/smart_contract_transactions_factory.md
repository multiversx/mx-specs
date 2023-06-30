## SmartContractTransactionsFactory

```
class SmartContractTransactionsFactory:
    // IConfig should be a private (internal) interface that defines the necessary configuration (e.g. minGasLimit, gasLimitPerByte, issueCost).
    constructor(config: IConfig);

    create_transaction_for_deploy

    create_transaction_for_execute

    create_transaction_for_upgrade

```
