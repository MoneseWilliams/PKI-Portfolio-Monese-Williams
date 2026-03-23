# Lab 05 — Extract a Certificate from a Live Website

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS:macOS
- Terminal used (Mac Terminal / Git Bash / WSL):Mac Terminal
- OpenSSL version ('OpenSSL 3.6.0'):
- Website used: tiktok.com

---

## Certificate Fields

| Field                    | Value from your output |
|--------------------------|------------------------|
| Subject                  |CN=*.tiktok.com         |
| Issuer                   |C=US, O=DigiCert Inc, OU=www.digicert.com, CN=RapidSSL TLS ECC CA G1|
| Not Before               |Jun 16 00:00:00 2025 GMT|
| Not After                |Jun 15 23:59:59 2026 GMT|
| Public Key Algorithm     |id-ecPublicKey          |
| Subject Alternative Name | DNS:*.tiktok.com, DNS:tiktok.com|
| Key Usage                |Digital Signature, Key Agreement|
| Extended Key Usage       |TLS Web Server Authentication, TLS Web Client Authentication|

---

## Observations

1. The organization this certificate belong to is CN=*.tiktok.com 
2. The certificate was issued by DigiCert Inc, specifically the RapidSSL TLS ECC CA G1 which is the intermediate certificate authority.
3. The certificate expires on June 15, 2026 at 23:59:59 GMT.
4. The domains listed in the SAN field are DNS:*.tiktok.com, DNS:tiktok.com
5. What is this certificate authorized to be used for? The certificate is authorized for TLS Web Server Authentication, TLS Web Client Authentication
