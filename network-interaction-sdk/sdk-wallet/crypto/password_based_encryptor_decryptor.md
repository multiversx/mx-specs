## PasswordBasedEncryptorDecryptor

```
class PasswordBasedEncryptorDecryptor:
    encrypt(data: bytes, password: string): PasswordEncryptedData;

    decrypt(data: PasswordEncryptedData, password: string): bytes;
```
