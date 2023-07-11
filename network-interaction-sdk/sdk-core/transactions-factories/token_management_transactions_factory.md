## TokenManagementTransactionsFactory

A class that provides methods for creating transactions for token management operations.

```
class TokenManagementTransactionsFactory:
    // IConfig should be a private (internal) interface that defines the necessary configuration (e.g. minGasLimit, gasLimitPerByte, issueCost).
    constructor(config: IConfig);

    create_transaction_intent_for_issuing_fungible({
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
    }): TransactionIntent;

    create_transaction_intent_for_issuing_semi_fungible({
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
    }): TransactionIntent;

    create_transaction_intent_for_issuing_non_fungible({
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
    }): TransactionIntent;

    create_transaction_intent_for_registering_meta_esdt({
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
    }): TransactionIntent;

    create_transaction_intent_for_registering_and_setting_roles({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        tokenType: RegisterAndSetAllRolesTokenType;
        numDecimals: number;
    }): TransactionIntent;

    create_transaction_intent_for_setting_burn_role_globally({
        sender: IAddress;
        tokenIdentifier: string;
    }): TransactionIntent;

    create_transaction_intent_for_unsetting_burn_role_globally({
        sender: IAddress;
        tokenIdentifier: string;
    }): TransactionIntent;

    create_transaction_intent_for_setting_special_role_on_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleLocalMint: boolean;
        addRoleLocalBurn: boolean;
    }): TransactionIntent;

    create_transaction_intent_for_setting_special_role_on_semi_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleNFTCreate: boolean;
        addRoleNFTBurn: boolean;
        addRoleNFTAddQuantity: boolean;
        addRoleESDTTransferRole: boolean;
    }): TransactionIntent;

    create_transaction_intent_for_setting_special_role_on_non_fungible_token

    create_transaction_intent_for_creating_nft

    create_transaction_intent_for_pausing

    create_transaction_intent_for_unpausing

    create_transaction_intent_for_freezing

    create_transaction_intent_for_unfreezing

    create_transaction_intent_for_wiping

    create_transaction_intent_for_local_minting

    create_transaction_intent_for_local_burning

    create_transaction_intent_for_updating_attributes

    create_transaction_intent_for_adding_quantity

    create_transaction_intent_for_burning_quantity

    ...
```
