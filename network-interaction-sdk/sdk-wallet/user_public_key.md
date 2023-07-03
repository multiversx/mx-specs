## UserPublicKey

```
class UserPublicKey:
    // The constructor(s) should also validate the length of the provided input.
    constructor(data: bytes);

    verify(data: bytes, signature: bytes): boolean;

    hex(): string {
        return this.buffer.toString("hex");
    }

    // Converts the public key to an actual address (bech32).
    toAddress(): Address;

    // Returns the secret key as a string (hex).
    // Name should be adjusted to match the language's convention.
    hex(): string;

    // Implement any of the following to return the secret key as a byte array.
    valueOf(): bytes;   // specific to JavaScript
    getData(): bytes;
```
