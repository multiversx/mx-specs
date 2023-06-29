## TokenTransfer

```
class TokenTransfer:
    // Fields (properties):
    token_identifier: ITokenIdentifier;
    token_nonce: INonce;
    amount_in_atomic_units: number;

    constructor(tokenIdentifier: ITokenIdentifier, tokenNonce: INonce, amountInAtomicUnits: number);

    isEgld(): boolean;

    isFungible(): boolean;

    // Named constructors:
    static ofEgld(amountInAtomicUnits: number): TokenTransfer;
    static ofFungible(tokenIdentifier: ITokenIdentifier, amountInAtomicUnits: number): TokenTransfer;
    static ofNonFungible(tokenIdentifier: ITokenIdentifier, nonce: INonce): TokenTransfer;
    static ofSemiFungible(tokenIdentifier: ITokenIdentifier, nonce: INonce, quantity: number): TokenTransfer;
    static ofMetaESDT(tokenIdentifier: ITokenIdentifier, nonce: INonce, amountInAtomicUnits: number): TokenTransfer;

    // Amount in atomic units, as string.
    toString();
```

Examples of usage:

```
amount = Amount.nativeFromUnits("1.50");
tokenTransfer = TokenTransfer.ofEgld(amount.toUnits());

print(tokenTransfer.isEgld());

transaction = new Transaction({
    ...
    value: amount, // compatible with ITransactionValue
    ...
});
```

```
amount = Amoount.fromUnits("10", 6);
tokenTransfer = TokenTransfer.ofFungible("TEST-abcdef", amount.toAtomicUnits());

print(tokenTransfer.isFungible());

transaction = tokenTransfersFactory.create_esdt_transfer({ 
    ...
    tokenTransfer: tokenTransfer,
    ...
 });
```
