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

Looking at the certificate chain, I was able to see that the leaf certificate for *.badssl.com was issued by the intermediate CA, COMODO RSA Domain Validation Secure Server CA. This intermediate CA was then issued a certificate by another CA, COMODO RSA Certification Authority. The last certificate in the chain was issued by AddTrust External CA Root, which is the root CA of the chain. This validates that the chain isn’t broken and has no errors, as each issued certificate leads up to a trusted CA and is not the reason behind the failed TLS connection.

**What you found:**

While validating the chain, I was able to verify that the chain is intact, with each certificate properly issued by the next authority all the way up to a trusted root CA.


---

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"
```

**What you found:**

When checking the revocation and trust of the certificate, I was able to confirm that the OCSP URL is present. I attempted to check the revocation status using OCSP, but received a “Responder Error: unauthorized (6),” indicating that the responder rejected the request and not showing a valid status of the certificate.

---

## Reflection

This lab reinforced a lot for me, not only is it helping me explain what I’m doing in technical terms better, but being able to break down the reason for the TLS failure to explaining the steps of remediation is key. A step where I really had to slow down at was step 4, revocation and trust. Querying the OCSP responder took some time since I had to create a directory to store my artifacts in for the leaf cert and the issuer cert, then query the responder, but I forgot to make the OCSP URL a variable so the txt file was empty, but once that was fixed I received the Responder Error: unauthorized (6), which isn’t an error I did something wrong but more of an error of the responder rejecting the request, but it did cause me to think carefully as well.

---

## Root Cause

The TLS failure was caused due to a certificate problem. The issued certificate used during the TLS handshake to secure the system’s connection expired, which caused the system to lose trust.

## Remediation

Step-by-step path to resolve this incident:

1. First, a new CSR would need to be created and submitted to the public CA.
2. Next, the CA will sign and issue the new certificate to the organization’s system with an updated validity period.
3. Lastly, upon receiving the replacement certificate, it will then need to be deployed and used to reestablish the TLS secure connection again within the TLS handshake. After a successful deployment, the chain needs to be verified using OpenSSL. The connection should then be secured, and patients should no longer receive warnings when accessing their patient portals.

## Key Findings

As I navigated through this lab, some key findings I observed is that the certificate is the leaf cert of the chain and expired on April 12, 2015, which was the entire reason behind the TLS failure. The certificate did populate both the CRL and OCSP URL, which both can be used to check the revocation status of the certificate. The certificate chain was also ruled out as not the cause of the failure, but the root CA is not in my system’s trust store, and after some research I determined the root CA, AddTrust External CA Root, is obsolete and no longer used. However, this is also a simulation, so I still wouldn’t count that as being the cause of the TLS failure, but it could be why the OCSP responder rejected my request for revocation status of the certificate.


## Challenges / Troubleshooting

One challenge I faced during this lab was making my artifacts directory. For some reason, instead of moving the file expired_cert.pem, I instead created expired-cert.pem and moved it into my week 6 submissions. So when I did the cat command to view the expired cert, it was just an empty file, and I ended up being stuck in the file and wasn’t sure how to get out of it, but then ended up using Control + C and it took me back to my home directory. I also had an issue with my paths and learned that using ~/labs and /labs mean two different things. If I use ~/labs, it’s because I’m in my home directory trying to get to that path, but with just /labs, it’s used for if I’m already in the path.

## Artifacts

- expired_cert.pem
