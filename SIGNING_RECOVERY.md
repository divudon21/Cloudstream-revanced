# Initial release signing recovery

The initial release workflow generates a unique signing keystore on the runner.
It encrypts the keystore and its randomly generated password with AES-256-CBC
(PBKDF2, 600000 iterations), then wraps the encryption password with RSA-OAEP
(SHA-256). Only the encrypted backup is published with the release. The RSA
recovery private key was generated separately and must be preserved securely
outside this public repository. Never commit or publish that private key or the
recovered keystore/password. Losing the recovery key prevents future updates
signed with the same identity.

To recover the keystore, download the encrypted backup and use the private key
in a private environment:

```sh
umask 077
mkdir -p recovery && cd recovery
tar -xzf ../cloudstream-revanced-encrypted-signing-backup.tar.gz
openssl pkeyutl -decrypt -inkey /path/to/recovery-private.pem \
  -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256 \
  -in wrapped-password.bin -out backup-password.txt
openssl enc -d -aes-256-cbc -pbkdf2 -iter 600000 -in signing-material.enc \
  -out signing-material.tar.gz -pass file:backup-password.txt
tar -xzf signing-material.tar.gz
```

`revanced.jks` and `password.txt` are sensitive. The signing alias is `revanced`;
the same password is used for the keystore and key. Store them securely and use
the same key for subsequent releases; a fresh key cannot update the initial APK.
