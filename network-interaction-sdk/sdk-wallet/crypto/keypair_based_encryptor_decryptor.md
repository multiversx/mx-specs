## KeyPairBasedEncryptorDecryptor

```
class KeyPairBasedEncryptorDecryptor:
    encrypt(data: bytes, recipient_public_key: IPublicKey, auth_secret_key: ISecretKey): PublicKeyEncryptedData;

    decrypt(data: PublicKeyEncryptedData, decryptor_secret_key: SecretKey): data;
```
