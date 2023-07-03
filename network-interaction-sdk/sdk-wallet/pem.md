## PEM

Implementation examples:
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/pem.ts
 - https://github.com/multiversx/mx-sdk-py-wallet/blob/main/multiversx_sdk_wallet/pem_entry.py

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
