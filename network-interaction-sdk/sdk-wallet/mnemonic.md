
```
class Mnemonic:
    // At least one of the following constructors should be implemented.
    // The constructor(s) should also trim whitespace and validate the mnemonic.
    constructor(text: string);
    constructor(words: string[]);

    // Alternatively, named constructors can be used:
    static fromText(text: string): Mnemonic;
    static fromWords(words: string[]): Mnemonic;

    // Generates a random mnemonic.
    static generate(): Mnemonic;

    // Derives a secret key from the mnemonic.
    deriveKey(addressIndex: number = 0, password: string = ""): UserSecretKey;

    // Gets the mnemonic words.
    getWords(): string[];

    // Returns the mnemonic as a string.
    toString(): string;
}
```
