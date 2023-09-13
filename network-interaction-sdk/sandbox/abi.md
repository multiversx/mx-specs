## ABI

TBD: Definitions will be continued in a future PR.

### MxValue and implementations

```
interface MxValue {
    // Encodes the instance data into a byte array.
    encode_top_level(): bytes;
    // Encodes the instance data into a byte array.
    encode_nested(): bytes;
    // Populates the instance with the decoded data.
    decode_top_level(data: bytes);
    // Populates the instance with the decoded data.
    decode_nested(data: bytes);
}
```

The implementing library should provide a number of `MxValue` implementations, listed below.

Primitive types:

```
MxBool
MxInt8, MxInt16, MxInt32, MxInt64, MxUInt8, MxUInt16, MxUInt32, MxUInt64
MxBigInt, MxBigUInt
MxBytes
MxString
MxH256
MxAddress
MxTokenIdentifier
MxCodeMetadata
```

Generic types:

```
MxOption
MxList
MxArray
```

Field-bearing types:

```
MxStruct (named fields)
MxTuple (numbered fields)
MxEnumValue
```

Additionally, each implementing class should have 
 - a `get_*()` that returns the underlying value(s) as specifically as possible in the implementing language.
 - a `get_length()` method (or something similar, according to language conventions) that returns the number of elements in the value, if applicable.
 - other methods 

For example:

```
class MxInt32 implements MxValue {
    get_value(): int32;
}

class MxList implements MxValue {
    get_items(): List[MxValue];
    get_length(): int32;
}
```

### MxValuesFactory

```
interface MxValuesFactory {
    create_bool(value: boolean): MxBool;
    create_int8(value: number): MxInt8;
    create_int16(value: number): MxInt16;
    create_int32(value: number): MxInt32;
    create_int64(value: number): MxInt64;
    create_uint8(value: number): MxUInt8;
    create_uint16(value: number): MxUInt16;
    create_uint32(value: number): MxUInt32;
    create_uint64(value: number): MxUInt64;
    create_bigint(value: number): MxBigInt;
    create_biguint(value: number): MxBigUInt;
    create_bytes(value: bytes): MxBytes;
    create_string(value: string): MxString;
    create_h256(value: bytes): MxH256;
    create_address(value: IAddress|IPublicKey|string): MxAddress;
    create_token_identifier(value: string): MxTokenIdentifier;

    create_code_metadata(
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
    ): MxCodeMetadata;

    create_option_provided(value: MxValue): MxOption;
    create_option_missing(): MxOption;
    create_list(values: List[MxValue]): MxList;
    create_array(values: List[MxValue]): MxArray;

}
```

The implementing library should provide the `V1MxValuesFactory` implementation of `MxValuesFactory`.

### AbiRegistry

`AbiRegistry` is an abstraction over the content of a `*.abi.json` file.

```
class AbiRegistry:
    // Will be defined in a future PR.
```

// struct definition
// enum definition

### TypeInferenceSystem
