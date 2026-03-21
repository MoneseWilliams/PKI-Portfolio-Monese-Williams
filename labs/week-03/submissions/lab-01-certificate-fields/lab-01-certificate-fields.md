# Lab 01 — Inspect X.509 Certificate Fields

## Overview
In this lab I was able to learn that X.509 certificates is the standard structure for digital certificates in PKI. I then implemented this knowledge using my local Mac terminal and OpenSSL to analyze the fields instead of relying on a web browser. X.509 certificates helps establish trust and identity within PKI systems

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
4. The public key algorithm used is id-ecPublicKey
5. The issuer field matters in a PKI system because it allows the browser to verify the certificate authority who issued and signed the certificate and use it to verify the trust chain up to the root 

---

## Stretch

I decided i wanted to go a step further and compare this Google certificate to a different website. The website I chose is Netflix

1. the issuer fields do not match since the issuer for Netflix is C=US, O=DigiCert Inc, CN=DigiCert Global G3 TLS ECC SHA384 2020 CA1 and Google is C=US, O=Google Trust Services, CN=WE2
2. Both certificates do use the same public key algorithm id-ecPublicKey
3. The validity periods for both certificates do not match. For Netflix's certificate expires Feb 18 21:41:48 2027 GMT and Googles certificate expires May 18 18:19:55 2026 GMT
4. The subject field are not structured the same way for Google it only populates as CN=*.google.com but for Netflix it populates as C=US, ST=California, L=Los Gatos, O=Netflix, CN=www.netflix.com. from my observation Netflix outputs more information for the subject field.  
