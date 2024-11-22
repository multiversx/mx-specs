## PublicKeyEncryptedData

```
dto PublicKeyEncryptedData:
    nonce: string;
    version: number;
    cipher: string;
    ciphertext: string;
    mac: string;
    identities: PublicKeyEncryptedDataIdentities;

dto PublicKeyEncryptedDataIdentities:
    recipient: string;
    ephemeralPubKey: string;
    originatorPubKey: string;
```
