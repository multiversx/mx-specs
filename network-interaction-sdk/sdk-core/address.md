```
class Address:
    // Should also validate the length of the provided input.
    constructor(pubkey: bytes, hrp: string);

    // Named constructors
    static fromBech32(value: string): Address;
    static fromHex(value: string, hrp: string): Address;

    // Returns the address as a string (bech32).
    bech32(): string;

    // Returns the address as a string (hex).
    // Name should be adjusted to match the language's convention.
    hex(): string;

    // Returns the underlying public key.
    pubkey(): bytes;

    // Returns the shard of the address.
    getShard(): int;

    // Returns true if the address is a smart contract address.
    isSmartContract(): bool;
```

Example of usage:

```
address = Address.from_bech32("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")

print("Address (bech32-encoded)", address.bech32())
print("Public key (hex-encoded):", address.hex())
```

```
class AddressFactory:
    constructor(hrp: string = DEFAULT_HRP);

    // Creates an address with all bytes set to zero.
    // This is the same as the "contract deployment address".
    createZero(): Address;

    // Creates an address from a bech32 string.
    createFromBech32(value: string): Address;

    // Creates an address from a public key.
    createFromPubkey(pubkey: bytes): Address;

    // Creates an address from a hex string.
    createFromHex(value: string): Address;
```

Example of usage:

```

factory = AddressFactory("erd")

address = factory.create_from_bech32("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")
address = factory.create_from_hex("0139472eff6886771a982f3083da5d421f24c29181e63888228dc81ca60d69e1")
address = factory.create_from_pubkey(bytes.fromhex("0139472eff6886771a982f3083da5d421f24c29181e63888228dc81ca60d69e1"))
```

```
class AddressConverter:
    constructor(hrp: string = DEFAULT_HRP);

    // Converts a bech32 string to a public key.
    bech32ToPubkey(value: string): bytes;

    // Converts a public key to a bech32 string.
    pubkeyToBech32(pubkey: bytes): string;
```

Example of usage:

```
converter = AddressConverter("erd")

pubkey = converter.bech32_to_pubkey("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")
bech32 = converter.pubkey_to_bech32(bytes.fromhex("0139472eff6886771a982f3083da5d421f24c29181e63888228dc81ca60d69e1"))
```

Module-level functions:

```
// Note that the first input parameter is received as an interface, but the return value is a concrete type [see Guideline **`in-ifaces-out-concrete-types`**].
compute_contract_address(deployer: IAddress, deployment_nonce: INonce, address_hrp: string): Address:
```
