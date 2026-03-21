# Lab 01 — Inspect X.509 Certificate Fields

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS:macOS
- Terminal used (Mac Terminal / Git Bash / WSL): Mac Terminal
- OpenSSL version (`OpenSSL 3.6.0')

---

## Certificate Fields

| Field                | Value from your output |
|----------------------|------------------------|
| Version              |  Version: 3            |
| Serial Number        | aa:23:02:42:8e:f4:39:7e:10:bb:2c:32:93:1c:fc:2e|
| Signature Algorithm  |ecdsa-with-SHA256       |
| Issuer               |C=US, O=Google Trust Services, CN=WE2|
| Subject              |CN=*.google.com         |
| Not Before           |Feb 23 18:19:56 2026 GMT|
| Not After            |May 18 18:19:55 2026 GMT|
| Public Key Algorithm |id-ecPublicKey          |

---

## Observations

1. This certificate was issued by C=US, O=Google Trust Services, CN=WE2
2. This certificate represents *.google.com
3. This certificate will expire May 18 18:19:55 2026 GMT
4. The public key algorithim used is id-ecPublicKey
5. The issuer field matters in a PKI system because it allows the browser to verify the certificate authority who issued and signed the certficate and use it to verify the trust chain up to the root 
