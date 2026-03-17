## Part 3 — Observations
- The verification succeeds before tampering because the file was never changed. So the digital signature still remained the same matching with the original data causing the verification to be successful.
- Verification fails after modification because the original file has been changed since it has been signed, so the signature no longer matches the data
- Digital signatures require both hashing and asymmetric cryptography because hashing helps determine whether the data has been modified, and asymmetric cryptography is used to verify who signed the data using a private and public key.
-This relates to certificate signing in PKI because certificate authorities issue certificates and digitally sign them using their private key. Systems then use the CA’s public key to verify who signed the certificate and confirm it is valid and trusted. Hashing is also involved to ensure the certificate has not been modified.
