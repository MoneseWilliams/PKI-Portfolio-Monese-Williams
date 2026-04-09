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

- [Key field or value from the certificate — e.g., Not After date, Issuer CN, SAN entries] When looking at the X.509 fields of the certificate, i was able to see the valdityy period of Not Before date: Apr  9 00:00:00 2015 GMT Not After date: Apr 12 23:59:59 2015 GMT confirming this certificate has expired April 12, 2015

- [Supporting command output or observation] Using the command 'openssl x509' i was able to parse and read the certificate fully. I observed the Subject, SAN, Issuer and Valditity fields during my diagnotic observation and was able to confirm that the Subject Alternative Name (SAN) DNS:*.badssl.com, DNS:badssl.com does match the Subject confirming their are no hostname mismatch. The issuer field also confirmed the certficate was issued by a public CA, however when looking at the valdity field i determined
- [Any additional evidence]

---

### Why it failed

[2–3 sentences: the technical explanation of the failure. Connect it to what you learned in the
relevant Week 5 or Week 6 lesson. Don't just describe what happened — explain why it caused a
TLS error.]

---

### Chain status

[Was the certificate chain structurally intact? Were there any chain-related issues separate from
the primary failure?]

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
[paste command here]
```

**What you observed:**

[What the output told you — connection errors, certificate retrieved, etc.]

---

### Step 2 — Parse

**Command used:**

```
[paste command here]
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN | |
| Issuer | |
| Not Before | |
| Not After | |
| SAN entries | |

**What you found:**

[What the parsed certificate told you about the failure]

---

### Step 3 — Validate the Chain

**Command used:**

```
[paste command here]
```

**Result:**

[Chain valid / chain broken — and what the error said]

**What you found:**

[What this step confirmed or ruled out]

---

### Step 4 — Check Revocation and Trust

**Command used:**

```
[paste command here]
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
