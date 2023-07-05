## EncryptedKeystore

```
class EncryptedKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Named constructor
    static new_from_secret_key(secret_key: ISecretKey): Keystore

    // Named constructor
    // Below, "wallet_provider" should implement "derive_secret_key_from_mnemonic()".
    // Advice: in the implementation all the parameters will be held as instance state (private fields).
    static new_from_mnemonic(wallet_provider: IWalletProvider, mnemonic: Mnemonic): Keystore

    // Importing "constructor"
    static import_from_object(wallet_provider: IWalletProvider, object: KeyfileObject, password: string): Keystore

    // Importing "constructor"
    static import_from_file(wallet_provider: IWalletProvider, path: Path, password: string): Keystore

    // When kind == 'secretKey', only index == 0 is supported.
    // When kind == 'mnemonic', the wallet provider is used under the hood to derive the secret key.
    // Below, "passphrase" is the bip39 passphrase required to derive a secret key from a mnemonic (by default, it should be an empty string).
    get_secret_key(index: int, passphrase: string): ISecretKey

    export_to_object(password: string): KeyfileObject

    export_to_file(path: Path, password: string)
```

```
dto KeyfileObject:
    version: number;

    // "secretKey|mnemonic"
    kind: string;
    
    // a GUID
    id: string

    // hex representation of the address
    address: string 

    // bech32 representation of the address
    bech32: string

    crypto: {
        // PasswordEncryptedData.ciphertext
        ciphertext: string;

        cipherparams: {
            // PasswordEncryptedData.iv
            iv: string;
        };

        // PasswordEncryptedData.cipher
        cipher: string;

        // PasswordEncryptedData.kdf
        kdf: string;
        kdfparams: {
            // PasswordEncryptedData.kdfparams.n
            n: number;

            // PasswordEncryptedData.kdfparams.r
            r: number;

            // PasswordEncryptedData.kdfparams.p
            p: number;

            // PasswordEncryptedData.kdfparams.dklen
            dklen: number;
            
            // PasswordEncryptedData.salt
            salt: string; 
        };

        // PasswordEncryptedData.mac
        mac: string;
    };
```

TBD: Can we perhaps adjust `EncryptedData` to be more compatible with `KeyfileObject.crypto`?

## Examples of usage

```
...
```
