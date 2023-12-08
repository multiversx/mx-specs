## ValidatorWalletProvider

```
class ValidatorWalletProvider:
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
```
