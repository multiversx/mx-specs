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

    // Can throw:
    // - ErrInvalidMnemonic
    derive_secret_key(address_index: number = 0, passphrase: string = ""): ISecretKey

    // Gets the mnemonic words.
    getWords(): string[];

    // Returns the mnemonic as a string.
    toString(): string;
}
```

## Examples of usage

Creating a new mnemonic and deriving the first secret key.

```
provider = new UserWalletProvider()
mnemonic = provider.generate_mnemonic()
print(mnemonic.toString())

mnemonic = Mnemonic.newfromText("...")
sk = provider.derive_secret_key_from_mnemonic(mnemonic, address_index=0 , passphrase="")
pk = provider.compute_public_key_from_secret_key(sk)
address = new Address(public_key, "erd")
print(address.bech32())
```
