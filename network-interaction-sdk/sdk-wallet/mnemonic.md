## Mnemonic

This component allows one to load / parse an existing mnemonic.

```
class Mnemonic:
    // At least one of the following constructors should be implemented.
    // The constructor(s) should also trim whitespace.
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

## MnemonicComputer

Encapsulates logic for generating and validating mnemonics and for deriving secret keys from mnemonics (i.e. BIP39).

```
class MnemonicComputer implements IMnemonicComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Should not throw.
    generate_mnemonic(): Mnemonic

    // Should not throw.
    validate_mnemonic(mnemonic: Mnemonic): bool

    // Can throw:
    // - ErrInvalidMnemonic
    // Below, "passphrase" is the optional bip39 passphrase used to derive a secret key from a mnemonic.
    // Reference: https://en.bitcoin.it/wiki/Seed_phrase#Two-factor_seed_phrases
    derive_secret_key_from_mnemonic(mnemonic: Mnemonic, address_index: int, passphrase: string = ""): ISecretKey
```

## Examples of usage

Creating a new mnemonic and deriving the first secret key.

```
computer = new MnemonicComputer()
provider = new UserWalletProvider()
mnemonic = computer.generate_mnemonic()
print(mnemonic.toString())

mnemonic = Mnemonic.newfromText("...")
sk = computer.derive_secret_key_from_mnemonic(mnemonic, address_index=0 , passphrase="")
pk = provider.compute_public_key_from_secret_key(sk)
address = new Address(public_key, "erd")
print(address.bech32())
```
