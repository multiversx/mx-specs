## UserWalletProvider

```
class UserWalletProvider:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, the constructor can be parametrized with underlying, more low-level crypto components, if applicable.

    // Should not throw.
    generate_keypair(): (ISecretKey, IPublicKey)

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

    // Should not throw.
    validate_mnemonic(mnemonic: Mnemonic): bool

    derive_secret_key_from_mnemonic(mnemonic: Mnemonic, address_index: int, passphrase: string): ISecretKey
```

Generally speaking, the following functions are only available in the `UserWalletProvider` (that is, they are not available in `ValidatorWalletProvider`):
 - `generate_mnemonic`
 - `validate_mnemonic`
 - `derive_secret_key_from_mnemonic`
