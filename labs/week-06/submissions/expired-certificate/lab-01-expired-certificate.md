# Lab 1 — Expired Certificate

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## Incident Summary

**Target system:** portal.metrogeneral.org (simulated via expired.badssl.com)
**Diagnosed by:** Monese Williams
**Date of diagnosis:** 4/8/2026

---

### What failed

The certifacte that was issued to the system had expired causing an unsecured TLS connection. 

---

### Evidence

-  When looking at the X.509 fields of the certificate, I was able to see the validity period of Not Before date: Apr 9 00:00:00 2015 GMT Not After date: Apr 12 23:59:59 2015 GMT confirming this certificate has expired April 12, 2015

-  Using the command 'openssl x509' I was able to parse and read the certificate fully. I observed the Subject, SAN, Issuer and Validity fields during my diagnostic observation and was able to confirm that the Subject Alternative Name (SAN) DNS:*.badssl.com, DNS:badssl.com does match the Subject confirming their is no hostname mismatch. The issuer field also confirmed the certificate was issued by a public CA (COMODO RSA Domain Validation Secure Server CA), however when looking at the validity field I determined the certificate was expired as of April 12, 2015
  
---

### Why it failed

- The TLS failure occurred due to an expired certificate that was not renewed on time. This caused the TLS handshake to fail because the certificate was no longer trusted by the system.
---

### Chain status

The certificate trust chain is valid with no related issues to the primary failure. Verifying the expired certifcate a the cause of the TLS failure. I validated the certificate chain and saw that the leaf certificate *.badssl.com was issued by the intermediate CA, COMODO RSA Domain Validation Secure Server CA. This intermediate CA was then issued a certificate by COMODO RSA Certification Authority, and the root CA was AddTrust External CA Root. This verifies that the chain is not broken and that each certificate is properly issued by the next authority.

---

### Remediation path

In order to fix this issue, first a new CSR needs to be created and submitted to the public CA. This will then allow the CA to sign and issue a new certificate with an updated validity period to the system. We then deploy the certificate to restore trust with the system and establish a secure TLS connection.

---

### Prevention

I recommend this organization create a Certificate Lifecycle Management (CLM) inventory list of all their certificates and implement automated renewal alerts using the 90, 60, 30 day rule to prevent missing renewal deadlines and risking certificates expiring. A 90 day alert should be set before the certificate expires, followed by a 60 day reminder to begin the renewal process, and lastly a 30 day reminder to complete the renewal process and monitor the deployment of the certificate.

---

## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

**Command used:** 

```
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | openssl x509 -outform PEM > expired_cert.pem
```

**What you observed:**

When retrieving the live certificate using the OpenSSL s_client command, the certificate was successfully returned and saved, with no errors shown during the connection.

---

### Step 2 — Parse

**Command used:**

```
openssl x509 -in expired_cert.pem -text -noout
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN |*.badssl.com|
| Issuer |COMODO RSA Domain Validation Secure Server CA|
| Not Before |Apr  9 00:00:00 2015 GMT|
| Not After |Apr 12 23:59:59 2015 GMT|
| SAN entries |DNS:*.badssl.com, DNS:badssl.com|

**What you found:**

After parsing the certificate and reading the X.509 certificate fields, it confirmed that the certificate was issued by a public CA. The Subject and SAN fields match the hostname, meaning there are no SAN mismatches. However, the certificate is expired, which would normally cause browsers to display a security warning, confirming the TLS failure was caused due to the certificate being expired.

---

### Step 3 — Validate the Chain

**Command used:**

```
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null
```

**Result:**

[Chain valid / chain broken — and what the error said] 

**What you found:**

[What this step confirmed or ruled out] Looking at the certificate chain, I was able to see that the leaf certificate for *.badssl.com was issued by the intermediate CA, COMODO RSA Domain Validation Secure Server CA. This intermediate CA was then issued a certificate by another CA, COMODO RSA Certification Authority. The last certificate in the chain was issued by AddTrust External CA Root, which is the root CA of the chain. This verifies that the chain is not broken and that each certificate is properly issued by the next authority


---

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"
```

**What you found:**

[OCSP URL present or absent, revocation status if checked, any trust store issues]

---

## Reflection

[2–3 sentences: What did this lab reinforce or clarify for you? Was there a step where
you had to slow down and think carefully?]

---

*CVI PKI Career Pathway — Foundations Phase*- Issuer:COMODO RSA Domain Validation Secure Server CA
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
