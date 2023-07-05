## PEMStore

```
class PEMKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Named constructor
    static new_from_secret_key(wallet_provider: IWalletProvider, secret_key: ISecretKey): PEMKeystore

    // Named constructor
    static new_from_secret_keys(wallet_provider: IWalletProvider, secret_keys: ISecretKey[]): PEMKeystore

    // Importing "constructor"
    static import_from_text(wallet_provider: IWalletProvider, text: string): PEMKeystore

    // Importing "constructor"
    static import_from_file(wallet_provider: IWalletProvider, path: Path): PEMKeystore

    get_secret_key(index: int): ISecretKey

    get_number_of_secret_keys(): number

    export_to_text(address_hrp: string): string

    export_to_file(path: Path, address_hrp: string)
```

## Examples of usage

Creating a PEM file to hold three newly created secret keys.

```
provider = new UserWalletProvider()
sk1, _ = provider.generate_keypair()
sk2, _ = provider.generate_keypair()
sk3, _ = provider.generate_keypair()

keystore = PEMKeystore.new_from_secret_keys(provider, [sk1, sk2, sk3])
keystore.export_to_file(Path("file.pem"), "erd")
```

Iterating over the accounts in a PEM file.

```
provider = new UserWalletProvider()
keystore = PEMKeystore.import_from_file(provider, Path("file.pem"))

for i in range(keystore.get_number_of_secret_keys()):
    sk = keystore.get_secret_key(i)
    pk = provider.compute_public_key_from_secret_key(sk)
    address = new Address(public_key, "erd")
    print("Address", i, address.bech32())
```
