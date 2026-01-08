## Interfaces

For languages that support **structural typing** (e.g. Go, Python, TypeScript), the following interfaces should not be _exported_, since we'd like to encourage client applications to define their own interfaces, as needed.

For languages that only support **nominal typing** (e.g. C#), these interfaces can be _exported_.

```
interface IWalletProvider:
    generate_keypair(): (ISecretKey, IPublicKey)
    sign(data: bytes, secret_key: ISecretKey): bytes
    verify(data: bytes, signature: bytes, public_key: IPublicKey): bool
    create_secret_key_from_bytes(data: bytes): ISecretKey
    create_public_key_from_bytes(data: bytes): IPublicKey
    compute_public_key_from_secret_key(secret_key: ISecretKey): IPublicKey
```

```
interface IMnemonicComputer:
    generate_mnemonic(): Mnemonic
    validate_mnemonic(mnemonic: Mnemonic): bool
    derive_secret_key_from_mnemonic(mnemonic: Mnemonic, address_index: int, passphrase: string): ISecretKey
```
