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

2. I identified the Root CA by checking the subject and issuer fields of the certificates and noticing that the true root is the one where the subject and issuer match, meaning it is self-signed. For this specifice certificate i had to dig deeper and find the correct Root Ca in keychain access and searching for the correct one since the 3 certficae block was issued by USERTrust ECC Certification Authority. Once searching and confirming this was in my trust systems trust store I verified that the subject and issuer did match verifying that this was the true Root CA
   
3. I was able to identify the intermediate Ca by checking the basic constraints and seeing its CA:TRUE meaning this CA can also issue certs but its not self signed like the Root Ca

5. The basic constraints field will provide the information on wether or not a certificate has the autorization to issue out certificates
   
6. Removing the intermediate certificate will break the chain because the system has to move up the chain and verify who issued certificates to who and is the issuer a valid certificate authority. If the intermediate ca is missing the system would be unable to verify who issued the cert to the leaf certificate since that helps link the leaaf cert to the root ca automatically failing the verification check

--

## Stretch

I wanted to see if verification would still work without the intermediate certificate, so I attempted to verify the certificate using OpenSSL. The output returned “server.pem: verification failed,” which I expected. This is because the intermediate certificate is required to complete the trust chain, as it links the leaf certificate to the root CA. Without it, the system cannot properly validate the certificate.

## Why This Matters

This matters because, in order to establish a secure TLS connection, the system must verify that the certificate issued to the server is valid. To do this, the system verifies that the certificate was issued by a trusted intermediate CA and then traces that CA back to a trusted root CA.
