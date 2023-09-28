## CustomToken

```
dto CustomToken:
    // E.g. "FOO-abcdef", "EGLD".
    identifier: string;
    nonce: number;
```

## TokenComputer

```
class CustomTokenComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Returns token.nonce == 0.
    // Can throw:
    // - ErrInvalidCustomTokenIdentifier
    is_fungible(token: CustomToken): boolean;

    // Given "FOO-abcdef-0a" returns 10.
    // Can throw:
    // - ErrInvalidCustomTokenIdentifier
    extract_nonce_from_extended_identifier(extended_identifier: string): number;

    // Given "FOO-abcdef-0a" returns "FOO-abcdef".
    // Can throw:
    // - ErrInvalidCustomTokenIdentifier
    extract_identifier_from_extended_identifier(extended_identifier: string): string;

    // Given "FOO-abcdef-0a" returns "FOO".
    // Given "FOO-abcdef" returns "FOO".
    // Can throw:
    // - ErrInvalidCustomTokenIdentifier
    extract_ticker_from_identifier(identifier: string): string;

    // Optionally, the implementing library can define a method for parsing all token parts at once.
    // The return type can be a tuple or the struct `CustomTokenIdentifierParts` (see below).
    // Can throw:
    // - ErrInvalidCustomTokenIdentifier
    parse_extended_identifier_parts(identifier: string): CustomTokenIdentifierParts;

    // Given "FOO-abcdef" and 10 returns "FOO-abcdef-0a". If nonce is 0, returns "FOO-abcdef".
    // For example, useful when preparing the parameters for querying token data from a network provider.
    compute_extended_identifier_from_identifier_and_nonce(identifier: string, nonce: number): string;

    // Optional, if `parse_extended_identifier_parts()` is also implemented. 
    // Pick one of the following (if the language does not support overloading):
    compute_extended_identifier_from_parts(ticker: string, random_sequence: string, nonce: number): string;
    compute_extended_identifier_from_parts(parts: CustomTokenIdentifierParts): string;
```

An optional structure, if the implementing library defines a `parse_extended_identifier_parts` method:

```
dto CustomTokenIdentifierParts:
    ticker: string;
    random_sequence: string;
    nonce: number;
```

## CustomTokenTransfer

```
dto CustomTokenTransfer:
    token: CustomToken;

    // Always in atomic units, e.g. for transferring 1.000000 "USDC-c76f1f", it must be "1000000".
    amount: Amount;
```
