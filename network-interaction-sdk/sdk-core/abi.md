## ABI

TBD: Definitions will be continued in a future PR.

### AbiRegistry

`AbiRegistry` is an abstraction over the content of a `*.abi.json` file.

```
class AbiRegistry:
    // Will be defined in a future PR.
```

### Codec

```
class Codec {
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with an AbiRegistry, when this is available for a given contract.
    // It follows that a Codec instance would be bound to a specific contract.

    encode_top_level(value: object): bytes;
    encode_nested(value: object): bytes;
    decode_top_level(data: bytes): object;
    decode_nested(data: bytes): object;
}
```
