## UserWalletProvider

```
class UserWalletProvider:
    // Should not throw.
    generate_pair(): (ISecretKey, IPublicKey)

    // Can throw:
    // - ErrInvalidSecretKey
    sign(data: bytes, secret_key: ISecretKey): bytes

    // Can throw:
    // - ErrInvalidPublicKey
    // - ErrInvalidSignature
    verify(data: bytes, signature: bytes, public_key: IPublicKey): bool

    // Can throw:
    // - ErrInvalidSecretKey
    create_secret_key_from_bytes(data: bytes): ISecretKey

    // Can throw:
    // - ErrInvalidPublicKey
    create_public_key_from_bytes(data: bytes): IPublicKey

    // Can throw:
    // - ErrInvalidSecretKey
    compute_public_key_from_secret_key(secret_key: ISecretKey): IPublicKey

    // Should not throw.
    generate_mnemonic(): Mnemonic

    // Can throw:
    // - ErrInvalidMnemonic
    derive_secret_key_from_mnemonic(mnemonic: Mnemonic, address_index: number = 0, passphrase: string = ""): ISecretKey
```

Generally speaking, `generate_mnemonic` and `derive_secret_key_from_mnemonic` are only available in the `UserWalletProvider` (that is, they are not available in `ValidatorWalletProvider`).
