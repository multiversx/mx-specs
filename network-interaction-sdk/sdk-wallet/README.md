## Keystores

Components in `sdk-wallet/keystore`, even if they do share a common purpose - storing secret keys - are not meant to be used interchangeably. Generally speaking, they do not (are not required to) share a common interface. If needed (for exotic use-cases), client code can design customized adapters over these components in order to unify their interfaces (this is not covered by specs).

**Implementation detail:** an instance of `EncryptedKeystore` (which is a wrapper over the well-known JSON wallet) holds decrypted data within its state.

**Design detail:** components in `sdk-wallet/keystore` should not depend on `Address` within their public interface (though they are allowed to depend on it within their implementation). For example, the `export` functionality of `EncryptedKeystore` requires the functionality provided by `Address` (conversion from public key bytes to bech32 representation).

### References:
 - https://github.com/ethereumjs/keythereum
 - https://github.com/ethereumjs/ethereumjs-wallet/blob/master/docs/classes/wallet.md
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/userWallet.ts
 - https://github.com/multiversx/mx-sdk-py-wallet/blob/main/multiversx_sdk_wallet/user_wallet.py
