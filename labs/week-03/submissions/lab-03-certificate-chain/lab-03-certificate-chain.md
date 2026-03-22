# Lab 03 — Verify a Certificate Chain

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS: macOS
- Terminal used (Mac Terminal / Git Bash / WSL):Mac Terminal
- OpenSSL version (`OpenSSL 3.6.0'):
- Website used: github.com

---

## Chain Verification Result
server.pem: OK

---

## Certificate Roles

| Certificate  | Role                        | Key Indicator                    |
|--------------|-----------------------------|----------------------------------|
| root.pem     | CA: TRUE                    |  Certificate Sign, CRL Sign      |
| intermediate.pem | CA: TRUE                | Digital Signature, Certificate Sign, CRL Sign|
| server.pem   | CA: FALSE                   | Digital Signature                            |

---

## Observations

1. After finding the correct Root Ca the chain did verify succussfully and the output said server.pem: OK verifying it was succesful
2. I identified the root CA by checking the subject and issuer fields of the certificates and noticing that the true root is the one where the subject and issuer match, meaning it is self-signed. I also confirmed it by checking Keychain Access and finding the certificate that sits at the top of the trust chain, by typing in the name of the issuer for the 3rd cert block and it populating USERTrust ECC CERTIFICATION AUTHORITY verifying that was the actual cert after getting 
3. How did you identify the intermediate CA?
4. What field confirms whether a certificate can issue other certificates?
5. Why does removing the intermediate certificate break the chain?
