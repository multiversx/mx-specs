## TokenManagementTransactionsFactory

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

    createRegisterMetaESDT({
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
