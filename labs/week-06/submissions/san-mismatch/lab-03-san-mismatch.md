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
  
- [Any additional evidence]

---

### Why it failed

The TLS failed due to a hostname mismatch. Since the organization changed the hostname to "staff.metrogeneral.org" but didn’t get a new certificate issued with the change included in the SAN DNS field, the server connection automatically failed due to the TLS handshake not being able to verify the subject’s name because it was not included.

---

### Chain status

[Was the certificate chain structurally intact? Were there any chain-related issues separate from
the primary failure?] When i first retrived the live certiface and seen the "Verify return code: 0 (ok)" this first confirmed that the certofcae was validated succuesfuly as well as the trsut chain bu of course you never want to assume but alwayes verify so when checking the certs trust chain manually 

---

### Remediation path

[Step-by-step: what needs to happen to restore the failing system? Be specific. Walk through
the process rather than summarizing it in one line.]

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

[What the output told you — connection errors, certificate retrieved, etc.]

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

[What the parsed certificate told you about the failure]

---

### Step 3 — Validate the Chain

**Command used:**

```
openssl verify mismatch_cert.pem
```

**Result:**

[Chain valid / chain broken — and what the error said]

**What you found:**

[What this step confirmed or ruled out]

---

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl x509 -in mismatch_cert.pem -noout -text | grep -A2 "OCSP"
```

**What you found:**

[OCSP URL present or absent, revocation status if checked, any trust store issues]

---

## Reflection

[2–3 sentences: What did this lab reinforce or clarify for you? Was there a step where
you had to slow down and think carefully?]

---

*CVI PKI Career Pathway — Foundations Phase*
