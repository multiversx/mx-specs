## UserPEM

`UserPEM` is a thin wrapper over the functionality of `pem`.

```
class UserPEM:
    constructor(label: string, key: UserSecretKey);

    // Named constructors:
    static from_file(path: Path, index: number = 0): UserPEM;
    static from_file_all(path: Path): UserPEM[];
    static from_text(text: string, index: number = 0): UserPEM;
    static from_text_all(text: string): UserPEM[];

    save(path: Path): void;
    to_text(): string;
```

### Implementation notes

 - https://github.com/multiversx/mx-sdk-py-wallet/blob/main/multiversx_sdk_wallet/user_pem.py
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/pem.ts
