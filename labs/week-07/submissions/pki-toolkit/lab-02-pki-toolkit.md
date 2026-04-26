# My PKI Toolkit
**CVI PKI Career Pathway — Phase 1 Foundations**
Completed: March 2026

---

## About This Document

[2–3 sentences describing what this document is and why you built it.]

---

## Core Command-Line Tools

### openssl x509

## openssl x509 — Parse the certificate

**What it does:**
This tool is used to parse an X.509 certificate so you can view important details such as certificate extensions, the issuer, and the subject fields.

**When to use it:**
It can be used for certificate analysis. It is also helpful for PKI diagnostic workflows when retrieving a live certificate and needing to view its details for troubleshooting.

**Example command from my labs:**

openssl x509 -in expired_cert.pem -text -noout

**What the output tells you:**
The output shows the issuer and subject information, the public key type, and the certificate extensions.

**Phase 1 source:** Week 7, Enterpise Certificate Analysis

### openssl s_client

## openssl s_client — Retrive Live Certificate

**What it does:**
This tool is used to retrieve a live certificate from a website or domain by connecting to the server and completing the TLS handshake.

**When to use it:**
It can be used in the event a certficates live cert needs to be pulled for a PKI diagnostic, 

**Example command from my labs:**

openssl x509 -in expired_cert.pem -text -noout

**What the output tells you:**
The output shows the issuer and subject information, the public key type, and the certificate extensions.

**Phase 1 source:** Week 7, Enterpise Certificate Analysis


### openssl req
[entry]

### openssl verify
[entry]

### openssl ocsp
[entry]

### openssl pkcs12
[entry]

[any additional openssl subcommands you used]

---

## Network and Inspection Tools

### curl
[entry]

[any others]

---

## Reference and Analysis Services

### SSL Labs SSL Server Test
[entry]

### crt.sh — Certificate Transparency Search
[entry]

---

## Phase 1 Skills Summary

[A brief paragraph — 3–5 sentences — summarizing what Phase 1 built as a whole and how these tools connect to real PKI operational work. Written in your own words.]

