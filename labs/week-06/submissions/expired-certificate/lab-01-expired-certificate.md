# Lab 01 — Diagnose an Expired Certificate

## Incident Summary

**Target System:** portal.metrogeneral.org (simulated via expired.badssl.com)

**Reported Behavior:** TLS failure — patients seeing security warnings when accessing the appointment portal

**Diagnostic Scope:** PKI Diagnostic Framework — all 4 steps

## Diagnostic Steps

**Step 1 — Retrieve: There are four steps in the PKI diagnostic framework that must be followed in order. Due to patients seeing security warnings when accessing the appointment portal, a diagnostic is needed. The first step is to obtain the live certificate from portal.metrogeneral.org. When retrieving the certificate using the OpenSSL s_client command, the certificate was successfully returned and saved with no errors were shown during the connection.

**Step 2 — Parse: After retreiving the live cert with no erros using openssl, i was then able to proceed to the next step of the diagonostic which is parse and reading the certifocate fields SAN, Issuer, and the Valdity dates using Openssl x509 -text -noout command. I was able to determine the  Issuer: C=GB, ST=Greater Manchester, L=Salford, O=COMODO CA Limited, CN=COMODO RSA Domain Validation Secure Server CA, Validity: Not Before: Apr  9 00:00:00 2015 GMT Not After : Apr 12 23:59:59 2015 GMT ( certificte expired Apr 13 2015) and X509v3 Subject Alternative Name: DNS:*.badssl.com, DNS:badssl.com 

**Step 3 — Validate the Chain:**

**Step 4 — Check Revocation and Trust:**

## Evidence

- Not Before date:
- Not After date:
- Days since expiration:
- Subject (entity the certificate was issued to):
- Issuer:
- Chain status (complete / incomplete):
- OCSP URL present? (yes/no):

## Root Cause

What caused the TLS failure? Be specific — is this a certificate problem, a chain problem, or a configuration problem?

## Remediation

Step-by-step path to resolve this incident:

1.
2.
3.

## Key Findings

## Challenges / Troubleshooting

## Artifacts

- expired_cert.pem
