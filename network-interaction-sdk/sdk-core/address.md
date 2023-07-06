## Address

```
class Address:
    // Should also validate the length of the provided input.
    // Can throw:
    // - ErrInvalidPublicKey
    constructor(public_key: bytes, hrp: string);

    // Named constructor
    // Can throw:
    // - ErrInvalidBech32Address
    static new_from_bech32(value: string): Address;

    // Named constructor
    // Can throw:
    // - ErrInvalidHexString
    static new_from_hex(value: string, hrp: string): Address;

    // Returns the address as a string (bech32).
    // Name should be adjusted to match the language's convention.
    to_bech32(): string;

    // Returns the address as a string (hex).
    // Name should be adjusted to match the language's convention.
    to_hex(): string;

    // Returns the underlying public key.
    get_public_key(): bytes;

    // Returns the human-readable part of the address.
    get_hrp(): string;

    // Returns true if the address is a smart contract address.
    is_smart_contract(): bool;
```

Example of usage:

```
address = Address.new_from_bech32("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")

print("Address (bech32-encoded)", address.bech32())
print("Public key (hex-encoded):", address.hex())
```

## AddressFactory

```
class AddressFactory:
    constructor(hrp: string = DEFAULT_HRP);

    // Creates an address with all bytes set to zero.
    // This is the same as the "contract deployment address".
    create_zero(): Address;

    // Creates an address from a bech32 string.
    create_from_bech32(value: string): Address;

    // Creates an address from a public key.
    create_from_public_key(public_key: bytes): Address;

    // Creates an address from a hex string.
    create_from_hex(value: string): Address;
```

Example of usage:

```

factory = AddressFactory("erd")

address = factory.create_from_bech32("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")
address = factory.create_from_hex("0139472eff6886771a982f3083da5d421f24c29181e63888228dc81ca60d69e1")
address = factory.create_from_public_key(bytes.fromhex("0139472eff6886771a982f3083da5d421f24c29181e63888228dc81ca60d69e1"))
```

## AddressComputer

```
class AddressComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Note that the first input parameter is received as an interface, but the return value is a concrete type (see guidelines).
    compute_contract_address(deployer: IAddress, deployment_nonce: INonce): Address;

    // The number of shards (necessary for computing the shard ID) would be received as a constructor parameter - constructor is not captured by specs.
    get_shard_of_address(address: IAddress): number;
```

Above, `IAddress` should satisfy:

```
get_public_key(): bytes;
get_hrp(): string;
```

OR, perhaps, it should simply satisfy:

```
// Name should be adjusted to match the language's convention.
to_bech32(): string;
```
