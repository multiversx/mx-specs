## Amount

```
class Amount:
    // Private fields (properties):
    atomic_units: number;
    num_decimals: number;

    constructor(atomic_units: number, num_decimals?: number = unknown);

    // Named constructors:
    fromAtomicUnits(atomic_units: number, num_decimals?: number = unknown): Amount;
    fromUnits(units: number|string, num_decimals: number): Amount;
    nativeFromUnits(units: number|string): Amount; // num_decimals = 18.

    // Simply returns the value of the private field.
    toAtomicUnits(): number;

    // Returns the fixed point representation of the amount.
    toUnits(): string;
```
