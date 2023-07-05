## Mnemonic

This component allows one to load / parse / validate an existing one. 

It also allows one to derive a secret key from the mnemonic.

```
class Mnemonic:
    // At least one of the following constructors should be implemented.
    // The constructor(s) should also trim whitespace and validate the mnemonic.
    constructor(text: string);
    constructor(words: string[]);

    // Alternatively, named constructors can be used:
    static newfromText(text: string): Mnemonic;
    static newfromWords(words: string[]): Mnemonic;

    // Gets the mnemonic words.
    getWords(): string[];

    // Returns the mnemonic as a string.
    toString(): string;
}
```
