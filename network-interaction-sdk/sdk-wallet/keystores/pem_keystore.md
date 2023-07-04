## PEMStore

```
class PEMKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Normally, the constructor should receive the following parameters: crypto provider, address HRP.

    // Named constructor
    static new_from_secret_key(secret_key: ISecretKey): PEMKeystore

    get_secret_key(index: int): ISecretKey

    // Returns the JSON representation of the keystore.
    serialize(): bytes

    save(path: Path)
```

## Examples of usage

```
...
```
