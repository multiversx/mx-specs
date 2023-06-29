## TokenManagementTransactionsFactory

A class that provides methods for creating transactions for token management operations.

```
class TokenManagementTransactionsFactory:
    create_transaction_for_issuing_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_issuing_semi_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    })

    create_transaction_for_issuing_non_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_registering_meta_esdt({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        numDecimals: number;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_registering_and_setting_roles({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        tokenType: RegisterAndSetAllRolesTokenType;
        numDecimals: number;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_setting_burn_role_globally({
        sender: IAddress;
        tokenIdentifier: string;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_unsetting_burn_role_globally({
        sender: IAddress;
        tokenIdentifier: string;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_setting_special_role_on_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleLocalMint: boolean;
        addRoleLocalBurn: boolean;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    create_transaction_for_setting_special_role_on_semi_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleNFTCreate: boolean;
        addRoleNFTBurn: boolean;
        addRoleNFTAddQuantity: boolean;
        addRoleESDTTransferRole: boolean;

        // Optionals:
        nonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    })

    ...
```

If the language supports it, the input arguments objects may be designed using interfaces and inheritance. For example, in TypeScript:

```
interface IRegisterMetaESDT extends IIssueSemiFungibleArgs {
    numDecimals: number;
}

interface IIssueSemiFungibleArgs extends IBaseArgs {
    issuer: IAddress;
    tokenName: string;
    tokenTicker: string;
    canFreeze: boolean;
    canWipe: boolean;
    canPause: boolean;
    canTransferNFTCreateRole: boolean;
    canChangeOwner: boolean;
    canUpgrade: boolean;
    canAddSpecialRoles: boolean;
}

interface IBaseArgs {
    nonce?: INonce;
    value?: ITransactionValue;
    gasPrice?: IGasPrice;
    gasLimit?: IGasLimit;
    guardian? : IAddress;
}
```
