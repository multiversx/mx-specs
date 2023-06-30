## ValidatorVerifier

```
class ValidatorVerifier:
    constructor(public_key: ValidatorPublicKey);

    // Named constructors
    static fromPublicKeyString(hex: string): ValidatorVerifier;

    verify(data: bytes, signature: bytes): boolean;

    getPublicKey(): ValidatorPublicKey;
```
