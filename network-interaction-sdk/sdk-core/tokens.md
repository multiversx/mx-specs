## Token

```
dto Token:
    // E.g. "FOO-abcdef", "EGLD".
    identifier: string;
    nonce: number;
```

## TokenComputer

```
class TokenComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Returns token.nonce == 0.
    is_fungible(token: Token): boolean;

    // Given "FOO-abcdef-0a" returns 10.
    // Can throw:
    // - ErrInvalidTokenIdentifier
    extract_nonce_from_extended_identifier(extended_identifier: string): number;

    // Given "FOO-abcdef-0a" returns "FOO-abcdef".
    // Can throw:
    // - ErrInvalidTokenIdentifier
    extract_identifier_from_extended_identifier(extended_identifier: string): string;

    // Given "FOO-abcdef-0a" returns "FOO".
    // Given "FOO-abcdef" returns "FOO".
    // Can throw:
    // - ErrInvalidTokenIdentifier
    extract_ticker_from_identifier(identifier: string): string;

    // Optionally, the implementing library can define a method for parsing all token parts at once.
    // The return type can be a tuple or the struct `TokenIdentifierParts` (see below).
    // Can throw:
    // - ErrInvalidTokenIdentifier
    parse_extended_identifier_parts(identifier: string): TokenIdentifierParts;

    // Given "FOO-abcdef" and 10 returns "FOO-abcdef-0a". If nonce is 0, returns "FOO-abcdef".
    // For example, useful when preparing the parameters for querying token data from a network provider.
    compute_extended_identifier_from_identifier_and_nonce(identifier: string, nonce: number): string;

    // Optional, if `parse_extended_identifier_parts()` is also implemented. 
    // Pick one of the following (if the language does not support overloading):
    compute_extended_identifier_from_parts(ticker: string, random_sequence: string, nonce: number): string;
    compute_extended_identifier_from_parts(parts: TokenIdentifierParts): string;
```

An optional structure, if the implementing library defines a `parse_extended_identifier_parts` method:

```
dto TokenIdentifierParts:
    ticker: string;
    random_sequence: string;
    nonce: number;
```

## TokenTransfer

```
dto TokenTransfer:
    token: Token;

    // Always in atomic units, e.g. for transferring 1.000000 "USDC-c76f1f", it must be "1000000".
    amount: Amount;
```
