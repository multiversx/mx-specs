# Specifications for mx-sdk-* libraries

This repository contains specifications for the `mx-sdk-*` libraries. The specifications are written in a language-agnostic manner, and are meant to be implemented in multiple languages (e.g. Go, TypeScript, Python, Rust, etc.).

## Structure

- `sdk-core`: core components (address, transaction, etc.).
- `sdk-wallet`: core wallet components (generation, signing).
- `sdk-network-providers`: Network Provider (API, Gateway) components.

## Guidelines

###  **`in-ifaces-out-concrete-types`**

Generally speaking, it's recommended to receive input parameters as abstractions (interfaces) in the public API of the SDKs. This leads to an improved decoupling, and allows for easier type substitution (e.g. easier mocking, testing).

Generally speaking, it's recommended to return concrete types in the public API of the SDKs. The client code is responsible with decoupling from unnecessary data and behaviour of returned objects (e.g. by using interfaces, on their side). The only notable exception to this is when working with factories (abstract or method factories) that should have the function output an interface type. For example, have a look over `(User|Validator)WalletProvider.generate_keypair()` - this method returns abstract types (interfaces).

### **`pay-attention-to-types`**

 - For JavaScript / TypeScript, `bytes` should be `Uint8Array`.

### **`follow-language-conventions`**
 
 - Make sure to follow the naming conventions of the language you're using, e.g. `snake_case` vs. `camelCase`.
 - In the specs, interfaces are prefixed with `I`, simply to make them stand out. However, in the implementing libraries, this convention does not have to be applied.
 - In `go`, the term `serialize` (whether it's part of a class name or a function name) can be replaced by `marshal`, since that is the convention.
 - Errors should also follow the language convention - e.g. `ErrInvalidPublicKey` vs `InvalidPublicKeyError`. Should have the same error message in all implementing libraries, though.

