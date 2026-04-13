# Lab 3 — San Mismatch

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## Incident Summary

**Target system:** wrong.host.badssl.com
**Diagnosed by:** Monese Williams
**Date of diagnosis:** 4/12/2026

---

### What failed

The TLS failure was caused by a hostname mismatch.

---

### Evidence

- When retrieving the live certificate, there was no hostname mismatch error, but there is a verify return code saying "Verify return code: 0 (ok)," meaning there are no certificate validation errors.

- After parsing the certificate and inspecting the X.509 certificate, I can confirm the certificate was issued by a public CA named R13. It also does not expire until June 22, 2026, meaning it is still valid. But, when checking the subject name "*.badssl.com" and the SAN field "DNS:*.badssl.com, DNS:badssl.com," I can confirm they both do match; however, with the organization changing their server name to "staff.metrogeneral.org" and it not being included in the SAN field, this could confirm why their server keeps receiving a TLS failure with a warning when attempting to access the site.
  
- [Any additional evidence

---

### Why it failed

The TLS failed due to a hostname mismatch. Since the organization changed the hostname to "staff.metrogeneral.org" but didn’t get a new certificate issued with the change included in the SAN DNS field, the server connection automatically failed due to the TLS handshake not being able to verify the subject’s name because it was not included.

---

### Chain status

When I first retrieved the live certificate and saw the "Verify return code: 0 (ok)," this first confirmed that the certificate was validated successfully as well as the trust chain. To further verify manually, I used the intermediate CA and the -untrusted flag and received an output, "mismatch_cert.pem: OK," verifying the SAN hostname mismatch is the primary failure of the TLS connection.

---

### Remediation path

A remediation path to restore the TLS failure would be to first request a new certificate to be reissued with the new hostname that was changed to staff.metrogeneral.org. After that, the new cert will need to be installed, then checked to see if the SAN field has the new hostname inside. After verifying successfully, deploy the new certificate within the server to fix the TLS failure.

---

### Prevention

[One concrete thing the organization could do differently to prevent this failure type from
recurring]

---

## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

**Command used:**

```
openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com \
  </dev/null 2>/dev/null | openssl x509 -outform PEM > mismatch_cert.pem

openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com </dev/null 2>&1
```

**What you observed:**

When I retrieved the live certificate from the server, I observed the output "Verify return code: 0 (ok)," which already tells me the certificate is validated with no errors within this first check.

---

### Step 2 — Parse

**Command used:**

```
openssl x509 -in mismatch_cert.pem -text -noout

openssl x509 -in mismatch_cert.pem -noout -text | grep -A5 "Subject Alternative Name"
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN |CN=*.badssl.com|
| Issuer |CN=R13|
| Not Before | Mar 24 20:02:52 2026 GMT|
| Not After |Jun 22 20:02:51 2026 GMT|
| SAN entries |DNS:*.badssl.com, DNS:badssl.com|

**What you found:**

When parsing this certificate, I was able to observe that the certificate is still valid due to not expiring until June 22, 2026, as well as the issuer being a public CA named R13. However, the subject and SAN fields do match, but with the change of the hostname and it not being a part of the SAN field, this does confirm the reasoning for the TLS failure.

---

### Step 3 — Validate the Chain

**Command used:**

```
openssl verify mismatch_cert.pem

 openssl verify -untrusted issuertwo_cert.pem mismatch_cert.pem
```

**Result:**

When validating the trust chain, I first received an error, "unable to get local issuer certificate" and "verification failed," because the leaf certificate was being validated without the intermediate CA being provided. I then downloaded the intermediate CA from the leaf cert and used the -untrusted flag to validate the trust chain manually, and after doing so I received an output, "mismatch_cert.pem: OK," verifying that the trust chain is structurally intact.

**What you found:**

This step helped rule out any broken chain issues that could have contributed to the cause of the TLS failure. Since there were no issues, this confirmed that the SAN mismatch is still the root cause of the TLS failure.

---

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl x509 -in mismatch_cert.pem -noout -text | grep -A2 "OCSP"
```

**What you found:**

When checking revocation and trust of this cert using the above command, there was no output or OCSP URL present, so no direct OCSP revocation status could be verified from the certificate itself. Revocation is also not relevant to this failure because there was no indication that the certificate was revoked, which also confirms the TLS failure is caused by the SAN hostname mismatch rather than any revocation issues.

---

## Reflection

This lab reinforced for me how important the SAN field is during the TLS handshake and how a certificate can still be fully valid but fail if the requested hostname is not included within the SAN. It also helped clarify the difference between certificate validity/trust chain issues with a missing cert and attempting to validate 

---

*CVI PKI Career Pathway — Foundations Phase*
