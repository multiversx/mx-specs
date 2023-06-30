## UserVerifier

```
class UserVerifier:
    constructor(public_key: UserPublicKey);

    // Named constructor
    fromAddress(address: IAddress): UserVerifier;

    verify(data: bytes, signature: bytes): boolean;

    getAddress(): Address;
```
