## TokenManagementTransactionsFactory

A class that provides methods for creating transactions for token management operations.

```
class TokenManagementTransactionsFactory:
    createIssueFungibleTransaction({
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
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createIssueSemiFungibleTransaction({
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
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    })

    createIssueNonFungibleTransaction({
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
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createRegisterMetaESDTTransaction({
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
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createRegisterAndSetAllRolesTransaction({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        tokenType: RegisterAndSetAllRolesTokenType;
        numDecimals: number;

        // Optionals:
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createSetBurnRoleGloballyTransaction({
        sender: IAddress;
        tokenIdentifier: string;

        // Optionals:
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createUnsetBurnRoleGloballyTransaction({
        sender: IAddress;
        tokenIdentifier: string;

        // Optionals:
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createSetSpecialRoleOnFungibleTransaction({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleLocalMint: boolean;
        addRoleLocalBurn: boolean;

        // Optionals:
        transactionNonce?: INonce;
        value?: ITransactionValue;
        gasPrice?: IGasPrice;
        gasLimit?: IGasLimit;
        guardian? : IAddress;
    }): Transaction;

    createSetSpecialRoleOnSemiFungibleTransaction({
        
    })
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
    transactionNonce?: INonce;
    value?: ITransactionValue;
    gasPrice?: IGasPrice;
    gasLimit?: IGasLimit;
    guardian? : IAddress;
}
```
