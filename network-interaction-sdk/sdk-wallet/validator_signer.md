## ValidatorSigner

```
class ValidatorSigner:
    constructor(secret_key: ValidatorSecretKey);

    // Named constructors
    static fromPemFile(path: Path, index: number = 0): ValidatorSigner;

    sign(data: bytes): bytes;

    getPublicKey(): ValidatorPublicKey;
```

