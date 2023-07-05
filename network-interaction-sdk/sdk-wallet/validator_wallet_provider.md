## ValidatorWalletProvider

```
class ValidatorWalletProvider:
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
```
