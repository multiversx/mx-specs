## TokenManagementTransactionIntentsFactory

A class that provides methods for creating transactions for token management operations.

```
class TokenManagementTransactionIntentsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", "issueCost", gas limit for specific operations etc. (e.g. "gasLimitForSettingSpecialRole").

    create_transaction_intent_for_issuing_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        initialSupply: int;
        numDecimals: int;
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

    create_transaction_intent_for_setting_special_role_on_non_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: str;
        addRoleNftCreate: bool;
        addRoleNftBurn: bool;
        addRoleNftUpdate_attributes: bool;
        addRoleNftAddUri: bool;
        addRoleESDTTransferRole: bool;
    }): TransactionIntent;

    create_transaction_intent_for_creating_nft({
        sender: IAddress;
        tokenIdentifier: str;
        initialQuantity: int;
        name: str;
        royalties: int;
        hash: str;
        attributes: bytes;
        uris: List[str];
    }): TransactionIntent;

    create_transaction_intent_for_pausing({
        sender: IAddress;
        tokenIdentifier: str;
    }): TransactionIntent;

    create_transaction_intent_for_unpausing({
        sender: IAddress;
        tokenIdentifier: str;
    }): TransactionIntent;

    create_transaction_intent_for_freezing({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: str;
    }): TransactionIntent;

    create_transaction_intent_for_unfreezing({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: str;
    }): TransactionIntent;

    create_transaction_intent_for_wiping({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: str;
    }): TransactionIntent;

    create_transaction_intent_for_local_minting({
        sender: IAddress;
        tokenIdentifier: str;
        supplyToMint: int;
    }): TransactionIntent;

    create_transaction_intent_for_local_burning({
        sender: IAddress;
        tokenIdentifier: str;
        supplyToBurn: int;
    }): TransactionIntent;

    create_transaction_intent_for_updating_attributes({
        sender: IAddress;
        tokenIdentifier: str;
        tokenNonce: int;
        attributes: bytes;
    }): TransactionIntent;

    create_transaction_intent_for_adding_quantity({
        sender: IAddress;
        tokenIdentifier: str;
        tokenNonce: int;
        quantityToAdd: int;
    }): TransactionIntent;

    create_transaction_intent_for_burning_quantity({
        sender: IAddress;
        tokenIdentifier: str;
        tokenNonce: int;
        quantityToBurn: int;
    }): TransactionIntent;

    ...
```
