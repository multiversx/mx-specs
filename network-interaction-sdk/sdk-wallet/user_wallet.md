## UserWallet

```
class UserWallet:
    constructor(kind: str, encrypted_data: EncryptedData, public_key_when_kind_is_secret_key: Optional[UserPublicKey])
    
    // Named constructor
    static from_secret_key(secret_key: UserSecretKey, password: str, randomness: Optional[IRandomness]): UserWallet
    static from_mnemonic(mnemonic: str, password: str, randomness: Optional[IRandomness]): UserWallet

    // Decryption (static) methods. 'keyfile_object' is the JSON object from the keyfile (wallet JSON).
    static decrypt_secret_key(keyfile_object: Any, password: str): UserSecretKey;
    static decrypt_mnemonic(keyfile_object: Any, password: str): Mnemonic;

    // Wrapper over the two methods above. Calls mnemonic.derive_key() under the hood.
    static load_secret_key(path: Path, password: str, address_index: Optional[int] = 0): UserSecretKey;

    // Saves the wallet to a file.
    save(path: Path, address_hrp: Optional[str]);

    // Converts the object to a JSON string (ready to be saved to a wallet keyfile).
    to_json(): str
```

### Implementation notes

 - https://github.com/multiversx/mx-sdk-py-wallet/blob/main/multiversx_sdk_wallet/user_wallet.py
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/userWallet.ts
