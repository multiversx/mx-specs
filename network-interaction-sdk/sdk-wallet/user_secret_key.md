```
class UserSecretKey:
    // At least one of the following constructors should be implemented.
    // The constructor(s) should also validate the length of the provided input.
    constructor(data: bytes);
    constructor(hex: string);

    // Alternatively, named constructors can be used:
    static fromBytes(data: bytes): UserSecretKey;
    static fromString(value: string): UserSecretKey;

    // Generates the public key corresponding to this secret key.
    generatePublicKey(): UserPublicKey;

    // Signs an array of bytes.
    sign(data: bytes): bytes;

    // Returns the secret key as a string (hex).
    // Name should be adjusted to match the language's convention.
    hex(): string;

    // Implement any of the following to return the secret key as a byte array.
    valueOf(): bytes;   // specific to JavaScript
    getData(): bytes;
}
```
