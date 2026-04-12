# Lab 2 — Broken Chain

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## Incident Summary

**Target system:** incomplete-chain.badssl.com
**Diagnosed by:** Monese Williams
**Date of diagnosis:** 4/12/2026

---

### What failed

[One sentence: what exactly caused the TLS failure] The TLS connection failed due to a server misconfiguration with a missing intermediate ca within the certificate trust chain.

---

### Evidence

-When inspecting the X.509 certificate fields, the Subject and the SAN fields matched, which crosses out any SAN hostname mismatches that can cause a TLS failure. The validity period also does not expire until June 22, 2026, meaning the certificate is still valid and not expired, and the leaf certificate was issued by a public CA, R13.

-When validating the certificate chain using openssl verify leaf_cert.pem, an error populated "unable to get local issuer certificate" and "verification failed," meaning the issuer certificate is missing from the trust chain, causing the trust chain to no longer be intact and officially broken with no way for the TLS handshake to establish a secure connection with the client. Verifying this would be the cause of the TLS failure.

-When retrieving the live certificate from incomplete-chain.badssl.com, an error did populate "unable to get local issuer certificate," verifying that even when attempting to obtain the live certificate of this server, the issuer certificate was missing from the chain, causing another error, "unable to verify the first certificate," to populate, meaning the leaf certificate is unable to be verified as well.

---

### Why it failed

The TLS connection failed because the certificate was renewed without the intermediate CA, causing a server misconfiguration. In order for TLS to establish a secure connection, the certificate trust chain must be fully intact. Since it was broken, the server was unable to verify the leaf certificate up to the root because the intermediate that issued the certificate to the leaf certificate was missing.
---

### Chain status

The certificate chain is not structurally intact, it was missing the intermediate CA that issued the certificate to the leaf certificate and helped link the root CA to it, which is primarily the cause of the TLS failure.

---

### Remediation path

A remediation path used to resolve the current TLS failure is by first connecting to the server and extracting the live certificate, then using the live leaf cert to obtain the URI and download the intermediate CA and convert it, if needed, from a DER to a PEM and use it to manually verify the chain. If successful, then it confirms the correct intermediate was found, and due to the certificate still being valid, the found intermediate CA will then need to be added to the chain and reconfigured to reestablish the connection within the TLS handshake so the failure will be resolved.

---

### Prevention

I recommend the organization implement a certificate validation procedure whenever certificates are renewed or replaced. For example, after installing the certificate, a verification of the full trust chain should be done using tools such as openssl s_client before fully deploying it to servers. This will help ensure that the intermediate CA is actually included within the certificate and prevent TLS failures caused by broken chains.

---

## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

**Command used:**

```
openssl s_client -connect incomplete-chain.badssl.com:443 </dev/null 2>&1
```

**What you observed:**

During step 1 of the PKI framework diagnostic, I was able to observe that the leaf certificate was successfully retrieved, but connection errors did populate within the process. For CN=*.badssl.com, the error "unable to get local issuer certificate" and "unable to verify the first certificate" populated, meaning the server is dealing with a misconfiguration and can primarily be the reason for the TLS failure. 

---

### Step 2 — Parse

**Command used:**

```
openssl x509 -in leaf_cert.pem -text -noout
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN |*.badssl.com|
| Issuer |CN=R13|
| Not Before |Mar 24 20:02:52 2026 GMT|
| Not After |Jun 22 20:02:51 2026 GMT|
| SAN entries |DNS:*.badssl.com, DNS:badssl.com|

**What you found:**

The parsed certificate has told me, when inspecting the X.509 certificate fields, that there is no hostname mismatch, the certificate is valid and doesn't expire until June 22, 2026, with the issuer being a public CA named R13. This parsing helped rule out any TLS failures that may be caused by hostname mismatch, certificate expiration, or an invalid issuer.

---

### Step 3 — Validate the Chain

**Command used:**

```
openssl verify leaf_cert.pem
openssl verify -untrusted issuer_cert.pem leaf_cert.pem
```

**Result:**

When validating the certificate chain, two errors, "unable to get local issuer certificate" and "verification failed," appeared, the same as in previous steps, confirming the chain is broken. I then went ahead and downloaded the intermediate CA from the leaf cert using the URI and converted it from a DER format to a PEM, saved it as issuer_cert.pem, and used the above command to verify if the chain would then show successful.

**What you found:**

This step confirmed that the certificate chain is broken and is the main cause of the TLS failure due to the missing intermediate CA and the server not being able to establish a secure connection because of it. After downloading the intermediate CA from the live cert and validating it again, an output showed "OK," verifying that the downloaded intermediate CA properly completes the trust chain.

The -untrusted flag when using the command "openssl verify -untrusted issuer_cert.pem leaf_cert.pem" is telling OpenSSL to use the downloaded intermediate CA when validating the leaf cert, but to not treat it as a trusted root CA.

---

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | grep "Verify return code"
```

**What you found:**

When doing step 4 of the PKI diagnosis, attempting revocation and trust using the above command, the live certificate still returned "Verify return code: 21 (unable to verify the first certificate)," which indicates the certificate chain is still incomplete. However, because validation did succeed manually using the downloaded intermediate CA and the -untrusted flag, this confirms that the root CA is trusted by the system and that the TLS failure is caused by the missing intermediate CA in the certificate chain.

---

## Reflection

This lab reinforced for me how important the certificate trust chain is during the TLS handshake and how even one missing intermediate CA can break the entire chain. It also helped me clarify the use of -untrusted in OpenSSL and how it helps validate a trust chain manually without treating it as a root to help confirm the missing intermediate that was downloaded will reestablish the TLS connection if used in a reissued certificate.

---

*CVI PKI Career Pathway — Foundations Phase*
