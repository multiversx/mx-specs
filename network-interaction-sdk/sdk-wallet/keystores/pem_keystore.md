## PEMStore

```
class PEMKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Named constructor
    static new_from_secret_key(secret_key: ISecretKey): PEMKeystore

    // The parameter "passphrase" would normally be ignored.
    get_secret_key(index: int, passphrase: string): ISecretKey

    // Should return false.
    supports_passphrase(): boolean

    // Returns the JSON representation of the keystore.
    serialize(): bytes

    save(path: Path)
```

## Examples of usage

```
...
```
