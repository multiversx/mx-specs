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
