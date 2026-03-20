# Lab — Digital Signatures

## Overview
The purpose of this lab was to understand how digital signatures work and how they provide integrity and authenticity. In this lab I generated an RSA key pair, signed a file using the private key, and then verified the signature using the public key. I also modified the file to see how tampering would cause the signature verification to fail.
This helps show the same concept used when certificate authorities sign certificates in PKI

---

## Environment
Document the environment used to complete the lab.

- Operating System: macOS
- Terminal Used: Terminal
- OpenSSL Version (if applicable): OpenSSL 3.6.0

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

1.  I first created a folder inside the root of my directory to store my artifacts for my digital signature lab and then created a test file that would be signed
2.  I then generated an RSA private key using OpenSSL aand then extracted the public key from it
3.  I then signed the test file using the private key and the SHA-256 hashing algorithim, which created a digital signature file
4.  I then verified the signature using the public key to confirm the file had not been altered.
5.  I then modified the filed and ran the verification again to observe that the signature verification failed after the file was changed 

---

## Results

The digital signature was successfully created using the private key. When i verified the signature using the public key before modifying the file, the output then returned back as "Verified OK". After modifying the file by adding additional text to it, i ran the verification again and the output returned a verification failure. This confirmed that the signature no longer matched the modified data.

![Digital Signature](assets/screenshots/week-02/digital-signatures.png) 

---

## Key Findings

•  A digital signature verifies both the integrity of the data and the identity of the signer.
•  The private key is used to create the signature, while the public key is used to verify it.
•  If the file is modified after being signed, the signature verification fails.

---

## Explanation

These results matter because digital signatures are a core part of PKI systems. They ensure that data has not been altered and confirm who signed the data. Certificate authorities use digital signatures to sign certificates so that systems can verify that the certificate is valid and trusted. If any part of the signed data changes, the verification process will fail, which protects against tampering.

---

## Challenges / Troubleshooting

One issue encountered during the lab was making sure the correct file paths were used when running the OpenSSL commands. If the command referenced the wrong location, the verification would not work properly. This was resolved by confirming the directory structure and ensuring the commands referenced the correct files.

---

## Artifacts

-artifact.txt
-artifact.sig
-public_key.pem

---

CVI PKI Career Pathway — Foundations Phase
