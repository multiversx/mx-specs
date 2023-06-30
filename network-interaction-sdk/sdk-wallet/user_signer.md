## UserSigner

```
class UserSigner:
    constructor(secret_key: UserSecretKey);

    // Named constructors
    static fromPemFile(path: Path, index: number = 0): UserSigner;
    static fromWallet(path: Path, password: string): UserSigner;

    sign(data: bytes): bytes;

    getAddress(): Address;
```
