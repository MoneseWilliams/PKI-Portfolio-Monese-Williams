# PKI Troubleshooting Runbook

---

## Title

TLS Failure Due to SAN Hostname Mismatch

---

## Problem Statement

I expected the TLS connection to complete successfully when connecting to the server, but instead the connection failed due to a hostname mismatch error.

---

## Environment

**Tool(s) used:**

- OpenSSL
- Terminal

**Certificate type or component involved:**

- TLS Certificate
- Subject Alternative Name (SAN)

**Any relevant configuration details:**

- Server hostname was changed but the certificate SAN field was not updated to include the new hostname

---

## Symptoms

- Browser warning 
- TLS connection fails even though certificate appears valid
- Certificate shows valid issuer and dates but still not trusted

---

## Diagnostic Steps

1. Retrieve the live certificate using (openssl s_client -connect [hostname]:443 2>/dev/null | openssl x509 -out [output_filename.pem]) and observe whether the certificate is retrieved successfully and whether the TLS connection shows any obvious errors.

2. Parse the certificate using (openssl x509 -in filename.pem -text -noout) and verify if the certificate is valid and not expired. Also check the Subject and SAN fields.

3. Inspect the SAN field specifiaclly using (openssl x509 -in filename.pem -noout -text | grep -A5 "Subject Alternative Name") to confirm whether the requested hostname appears in the SAN field. If the hostname is missing, this confirms a hostname mismatch.

4. Validate the chain using (openssl verify filename.pem) to confirm whether the certificate chain is valid or if the issue is related to trust chain validation.

5. query the OCSP responder and check certficate revocation status using (openssl x509 -in [filename.pem] -text -noout | grep -A4 "Authority Information Access") to confirm revocation is not causing an additional issue with the TLS failure.
   
---

## Resolution

The issue was resolved by requesting and issuing a new certificate that includes the correct hostname in the SAN field. Once the updated certificate was installed on the server, the TLS connection was successfully established.

---

## Prevention Note

Always update and verify the SAN field when making hostname changes. Certificates should be reissued whenever a new hostname is introduced to avoid TLS failures.

---

**Lab this scenario is drawn from:** `labs/week-06/submissions/san-mismatch/...`

---

*CVI PKI Career Pathway — Phase 1 Foundations*
