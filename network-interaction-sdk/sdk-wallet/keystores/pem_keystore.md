## PEMStore

```
class PEMKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Named constructor
    static new_from_secret_key(secret_key: ISecretKey): PEMKeystore

    // Named constructor
    static new_from_secret_keys(secret_keys: ISecretKey[]): PEMKeystore

    // Importing "constructor"
    static import_from_text(text: string): PEMKeystore

    // Importing "constructor"
    static import_from_file(path: Path): PEMKeystore

    get_secret_key(index: int): ISecretKey

    export_to_text(): string

    export_to_file(path: Path)
```

## Examples of usage

```
...
```
