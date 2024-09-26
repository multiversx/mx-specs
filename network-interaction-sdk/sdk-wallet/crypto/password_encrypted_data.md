## PasswordEncryptedData

```
dto PasswordEncryptedData:
    id: string;
    version: number;
    cipher: string;
    ciphertext: string;
    iv: string;
    kdf: string;
    kdfparams: object;
    salt: string;
    mac: string;

// Example of class compatible with "PasswordEncryptedData.kdfparams".
// Can be anything, and it's specific to a "PasswordEncryptedData.kdf" (e.g. "scrypt").
// EncryptorDecryptor.encrypt() puts this into "PasswordEncryptedData.kdfparams".
// EncryptorDecryptor.decrypt() interprets (possibly validates) it.
dto ScryptKeyDerivationParams:
    // numIterations
    n = 4096;

    // memFactor
    r = 8;

    // pFactor
    p = 1;

    dklen = 32;
```
