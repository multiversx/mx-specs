## Keystores

Components in `sdk-wallet/keystore` should implement the interface:

```
get_secret_key(index: int, passphrase: string): ISecretKey
```

`PEMKeystore` would simply ignore the parameter `passphrase`. `EncryptedKeystore` created with a secret key (as opposed to a mnemonic) would also ignore the parameter `passphrase`.

**Implementation detail:** given the constraint above, it follows that an instance of `EncryptedKeystore` (which is a wrapper over the well-known JSON wallet) holds decrypted data within its state.

**Design detail:** components in `sdk-wallet/keystore` should not depend on `Address` within their public interface (though they are allowed to depend on it within their implementation). For example, the `export` functionality of `EncryptedKeystore` requires the functionality provided by `Address` (conversion from public key bytes to bech32 representation).

### References:
 - https://github.com/ethereumjs/keythereum
 - https://github.com/ethereumjs/ethereumjs-wallet/blob/master/docs/classes/wallet.md
