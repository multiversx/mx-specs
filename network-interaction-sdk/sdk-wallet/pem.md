## PEM

`PemEntry` is a simple tuple of `label` (e.g. bech32 address, BLS key) and `data` (user secret key, validator secret key). It is used handle the hold of PEM files.

Module-level functions are used to parse PEM files into `PemEntry` items.

```
class PemEntry:
    label: string;
    data: bytes;
```

Module-level functions:

```
parse_text(text: string): PemEntry[];
parse_file(path: Path): PemEntry[];
```

### Implementation notes

 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/pem.ts
 - https://github.com/multiversx/mx-sdk-py-wallet/blob/main/multiversx_sdk_wallet/pem_entry.py
