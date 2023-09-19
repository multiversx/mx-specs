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
    is_fungible_token(token: Token): boolean;

    // Given "FOO-abcdef-0a" returns 10.
    extract_token_nonce_from_extended_identifier(extended_identifier: string): number;

    // Given "FOO-abcdef-0a" returns "FOO-abcdef".
    extract_token_identifier_from_extended_identifier(extended_identifier: string): string;

    // Given "FOO-abcdef-0a" returns "FOO".
    // Given "FOO-abcdef" returns "FOO".
    extract_token_ticker_from_identifier(identifier: string): string;
```

## CustomTokenTransfer

```
dto CustomTokenTransfer:
    token: CustomToken;

    // Always in atomic units, e.g. for transferring 1.000000 "USDC-c76f1f", it must be "1000000".
    amount: Amount;
```
