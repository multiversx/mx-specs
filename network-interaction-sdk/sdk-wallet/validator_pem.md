```
class ValidatorPEM:
    constructor(label: string, key: ValidatorSecretKey);

    // Named constructors:
    static from_file(path: Path, index: number = 0): ValidatorPEM;
    static from_file_all(path: Path): ValidatorPEM[];
    static from_folder_all(path: Path): ValidatorPEM[];
    static from_text(text: string, index: number = 0): ValidatorPEM;
    static from_text_all(text: string): ValidatorPEM[];

    save(path: Path): void;
    to_text(): string;
```
