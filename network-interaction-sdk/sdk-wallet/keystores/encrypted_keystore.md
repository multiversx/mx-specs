## EncryptedKeystore

```
class EncryptedKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Normally, the constructor should receive the following parameters: crypto provider, address HRP.

    // Named constructor
    static new_from_secret_key(secret_key: ISecretKey, password: string): Keystore
    
    // Named constructor
    // In the future, "Trezor" password can be provided, as well.
    static new_from_mnemonic(mnemonic: Mnemonic, password: string): Keystore
    
    // Named constructor
    static new_from_file(path: Path, password: string): Keystore

    // When kind == 'secret_key', only index == 0 is supported.
    // When kind == 'mnemonic', the crypto provider is used under the hood to derive the secret key.
    get_secret_key(index: int): ISecretKey

    // Returns the JSON representation of the keystore.
    serialize(): bytes

    save(path: Path)
```

## Examples of usage

```
...
```
