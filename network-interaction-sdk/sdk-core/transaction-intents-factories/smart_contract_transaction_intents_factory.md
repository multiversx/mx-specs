## SmartContractTransactionIntentsFactory

```
class SmartContractTransactionIntentsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte" etc.
    // The constructor may also be parametrized with a Codec instance, necessary to encode contract call arguments.

    create_transaction_intent_for_deploy({
        sender: IAddress;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        // Question for review: is this a valid case?
        native_transfer_amount: Amount = 0;
        custom_transfers: List[CustomTokenTransfer] = [];
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
    }): TransactionIntent;

    create_transaction_intent_for_execute({
        sender: IAddress;
        contract: IAddress;
        // If "function" is a reserved word in the implementing language, it should be replaced with a different name (e.g. "func" or "functionName").
        function: string;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        custom_transfers: List[CustomTokenTransfer] = [];
        gasLimit: uint32;
    }): TransactionIntent;

    create_transaction_intent_for_upgrade({
        sender: IAddress;
        contract: IAddress;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        // Question for review: is this a valid case?
        native_transfer_amount: Amount = 0;
        custom_transfers: List[CustomTokenTransfer] = [];
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
    }): TransactionIntent;

    ...

```
