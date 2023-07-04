##  **`in-ifaces-out-concrete-types`**

Generally speaking, it's recommended to receive input parameters as abstractions (interfaces) in the public API of the SDKs. This leads to an improved decoupling, and allows for easier type substitution (e.g. easier mocking, testing).

Generally speaking, it's recommended to return concrete types in the public API of the SDKs. The client code is responsible with decoupling from unnecessary data and behaviour of returned objects (e.g. by using interfaces, on their side). 

## **`pay-attention-to-types`**

 - For JavaScript / TypeScript, `bytes` should be `Uint8Array`.

## **`follow-language-conventions`**
 
 - Make sure to follow the naming conventions of the language you're using, e.g. `snake_case` vs. `camelCase`.
 - In the specs, interfaces are prefixed with `I`, simply to make them stand out. However, in the implementing libraries, this convention does not have to be applied.
 - In `go`, the term `serialize` (whether it's part of a class name or a function name) can be replaced by `marshal`, since that is the convention.

## **`out-of-specs-arguments-should-be-optional`**

E.g. randomness.
