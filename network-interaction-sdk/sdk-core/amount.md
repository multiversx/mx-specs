## Amount

Generally speaking, the `Amount` type should be:
 - `int` in Python
 - `big.Int` or `string` in Go
 - `BigNumber.Value` (which includes `string`) in JavaScript

The implementing library can define type aliases, if desired, for example:

```Python
Amount = int
```

```Go
type Amount = big.Int
```

```JavaScript
type Amount = BigNumber.Value
```

Furthermore, the implementing library is free to define a wrapper class or structure. This isn't a requirement though. Example:

```
class Amount:
    value: (big.Int|bigNumber|string)
    to_string(): string
```

Within the specs, we use `Amount` and we mean `(bigNumber|string)`. Or, to put it differently, `(Go[big.Int]|Python[int]|JavaScript[BigNumber.Value|string])`.

**Important:** 
 - the value of an `Amount` is **always expressed in atomic units**. For example, to represent 1 EGLD, the value of `Amount` should be `1000000000000000000`, whether it's a Go `big.Int`, a Python `int`, a `string` etc.
 - `Amount` **must not be concerned** with the number of decimals of the token. Just as the Protocol itself isn't concerned with this.

## AmountInUnits

Generally speaking, the `AmountInUnits` type should always be a `string`, so that **computations based on `AmountInUnits` are discouraged**. This type is meant to be used in the higher layers of an application, for example, on display purposes.

The implementing library can define type aliases or wrapper classes (structures), if desired. See above.

## AmountComputer

```
class AmountComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, the constructor can be parametrized with the decimals separator (e.g. "." or ",") and with the number of decimals of the native token (18).

    amount_to_units(amount: Amount, num_decimals: number): AmountInUnits;
    units_to_amount(units: AmountInUnits, num_decimals: number): Amount;

    native_amount_to_units(amount: Amount): AmountInUnits;
    native_units_to_amount(units: AmountInUnits): Amount;
```
