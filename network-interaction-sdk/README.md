# Specifications for mx-sdk-* libraries

This repository contains specifications for the `mx-sdk-*` libraries. The specifications are written in a language-agnostic manner, and are meant to be implemented in multiple languages (e.g. Go, TypeScript, Python, Rust, etc.).

## Structure

- `sdk-core`: core components (address, transaction, etc.).
- `sdk-wallet`: core wallet components (generation, signing).
- `sdk-network-providers`: Network Provider (API, Gateway) components.

Below, we add specific details for some of the most important packages and sub-components.

### Transactions Factories

These components are located in `sdk-core/transactions-factories` and are responsible with creating transactions for specific use cases. They are designed as _multi-factory_ classes, having methods that return a `Transaction` object constructed by following specific recipes (with respect to the Protocol). 

The methods are named in correspondence with the use cases they implement, e.g. `create_transaction_for_native_transfer()` or `create_transaction_for_new_delegation_contract()`. They return a `Transaction` (data transfer object), where `sender`, `receiver`, `value`, `data` and `gasLimit` are properly set (upon eventual computation, where applicable).

Optionally, the implementing library can choose to return an object that isn't a complete representation of the `Transaction`, if desired. In this case, the library must name the incomplete representation `DraftTransaction`, and also must provide a direct conversion facility from `DraftTransaction` to `Transaction` - for example, a named constructor. See [transaction](sdk-core/transaction.md).

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

## **`any-object`**

In the specs, `object` is used as a placeholder for any type of object. In the implementing libraries, this would be replaced with the most appropriate type. For example:

- in Go:
  - before Go 1.18: `map[string]interface{}` or directly `interface{}` (depending on the context)
  - after Go 1.18: `map[string]any` or directly `any` (depending on the context)
- in Python: `dict` or `Any`
- in JavaScript / TypeScript: `object` or `any`

