# Lab 01 — Diagnose an Expired Certificate

## Incident Summary

**Target System:** portal.metrogeneral.org (simulated via expired.badssl.com)

**Reported Behavior:** TLS failure — patients seeing security warnings when accessing the appointment portal

**Diagnostic Scope:** PKI Diagnostic Framework — all 4 steps

## Diagnostic Steps

**Step 1 — Retrieve: There are four steps in the PKI diagnostic framework that must be followed in order. Due to patients seeing security warnings when accessing the appointment portal, a diagnostic is needed. The first step is to obtain the live certificate from portal.metrogeneral.org. When retrieving the certificate using the OpenSSL s_client command, the certificate was successfully returned and saved with no errors were shown during the connection.

**Step 2 — Parse: After retriveing the live certifiacte with no erros using OpenSSL, i was able to proceed to the next step of the diagnostic, which is parsing and reading the certificate fields such as SAN,Issuer, Subject, and the vvalidity date using the openssl c509 -text -noout command

I was able to detrmine the following:
- Issuer: CN=COMODO RSA Domain Validation Secure Server CA
- Validity: Not Before: Apr 9 00:00:00 2015 GMT, Not After: Apr 12 23:59:59 2015 GMT (certificate expired April 12, 2015)
- Subject Alternative Name (SAN): DNS:*.badssl.com, DNS:badssl.com
- Subject: CN=*.badssl.com

These certificate fields confirm that the certifiacte was issued by a public CA. The Subject and SAN fields hostname match, meanthing thier are no SAN mismatcch. However the certificate is expired, which normally would cause browsers to display a security warning.

**Step 3 — Validate the Chain: Even though a cause to the issue may have been identified in step 2, it is still important to complete the full diagnostic framework. This brings me to step 3, which is validating the certificate chain and confirming that the full chain from the leaf to the root is intact using OpenSSL.

Looking at the certificate chain, I was able to see that the leaf certificate for *.badssl.com was issued by the intermediate CA, COMODO RSA Domain Validation Secure Server CA. This intermediate CA was then issued a certificate by another CA, COMODO RSA Certification Authority. The last certificate in the chain was issued by AddTrust External CA Root, which is the root CA of the chain. This verifies that the chain is not broken and that each certificate is properly issued by the next authority

**Step 4 — Check Revocation and Trust: 

## Evidence

- Not Before date: Apr  9 00:00:00 2015 GMT
- Not After date: Apr 12 23:59:59 2015 GMT
- Days since expiration: 4018
- Subject (entity the certificate was issued to):
- Issuer:COMODO RSA Domain Validation Secure Server CA
- Chain status (complete / incomplete): complete
- OCSP URL present? (yes/no): Yes 

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
