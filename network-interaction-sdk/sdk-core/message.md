## Message

```
dto Message:
    data: bytes;
    signature: bytes;
```

## MessageComputer

```
class MessageComputer:
    compute_bytes_for_signing(message: Message): bytes;
```

Reference implementation of `compute_bytes_for_signing`:

```
compute_bytes_for_signing(message: Message):
    PREFIX = bytes.fromhex("17456c726f6e64205369676e6564204d6573736167653a0a")
    size = length of message.data, encoded as a string
    content = concat(PREFIX, size, message.data)
    content_hash = keccak(digest_bits=256)
    
    return content_hash
```
