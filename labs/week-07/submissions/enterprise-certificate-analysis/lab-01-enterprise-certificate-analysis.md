# Lab — Enterprise Certificate Analysis

## Overview
In this lab, I will be retrieving a live certificate from a well-known enterprise organization and performing a complete certificate analysis on the TLS certificate and its full deployment. I will be investigating where TLS terminates within the enviroment and documenting my findings.

---

## Steps Performed

1. First, I will be retrieving the live certificate from my target enterprise, Capital One Bank, using openssl s_client. I then viewed the live certificate to confirm it populated successfully and decided to pull up the full certificate chain for Capital One Bank. The full certificate TLS handshake completed succuessfully and populated three certificate blocks.

2. Next, I parsed the live certificate and inspected it, recording the validity field: Not Before: Jan 9 00:00:00 2026 GMT and Not After: Jan 8 23:59:59 2027 GMT, with approximately 364 days remaining before expiration. The Subject field shows CN=capitalone.com and O=Capital One Financial Corporation, which indicates the certificate type is OV (Organization Validation) due to the organization details it includes. In the SAN field, it only shows one entry: DNS:capitalone.com, with no wildcard entries. This suggests the certificate architecture is only valid for the domain capitalone.com. The Issuer is CN=DigiCert EV RSA CA G2, which is a known public CA, DigiCert.

3. Then, I went ahead and analyzed the full certificate chain. There are a total of three certificates in the chain. The leaf certificate is present, being the first certificate in the chain. The intermediate CA is also present, with the Subject being CN=DigiCert EV RSA CA G2. The chain then terminates at a public root CA named DigiCert Global Root G2, confirming that this chain is complete.

4. Next, I determined where TLS terminates and whether there were any CDN or load balancer indicators using OpenSSL. The issuer field suggests that this is not a CDN-managed certificate, but rather a known public CA named DigiCert, since the issuer name does not indicate any CDN providers. There are also no CDN-related SAN entries or wildcard patterns in the SAN field, only one domain: DNS:capitalone.com. The server headers indicate that a load balancer is in the request path and not a CDN, with the server output showing BigIP and no CDN indicators. Overall, based on these findings, I determined that TLS most likely terminates at the load balancer for this deployment.

5. Next, I decided to run an SSL analysis using https://www.ssllabs.com/ssltest/ for a complete TLS configuration report. The SSL Labs overall grade is an A+, meaning it has excellent security with strong configurations and supports TLS version 1.3. Deprecated TLS versions 1.0 and 1.1 are not supported under the Protocols tab. HSTS (HTTP Strict Transport Security) is configured, and OCSP stapling is also supported.

6. Lastly, I checked the certificate transparency logs to understand how long the organization has been using this CA and to see if there are any unexpected issuers. I used the site https://crt.sh, and after searching my chosen enterprise hostname, www.capitalone.com, I saw that there are approximately 100 certificates that have been issued for this domain. Recent issuers, such as CN=DigiCert EV RSA CA G2, have been consistent for this domain, but different CAs have also been used, including DigiCert Global G3 TLS ECC SHA384 2020 CA1, CN=DigiCert SHA2 Extended Validation Server CA, and Symantec Trust Network, CN=Symantec Class 3 EV SSL CA - G3. There are also some older or unfamiliar issuers in the CT logs, such as CN=Symantec Class 3 EV SSL CA - G3, which could indicate that this certificate authority is no longer in active use. The approximate validity period of the most recent certificates is around 1 to 2 years, since DigiCert is a paid CA and most paid certificates follow a 1 to 2 year validity pattern.

Certificate Summary: Issuer, validity window, certificate type (DV/OV/EV), SAN count, wildcard usage.

Chain Analysis: Number of certificates in chain, intermediate CA identity, root CA identity, chain completeness.

Termination Analysis: Where TLS appears to terminate (app server / load balancer / CDN) and the evidence that supports your conclusion.

TLS Configuration: SSL Labs grade, TLS versions, HSTS, OCSP stapling.

CT Log Analysis: CA consistency, any unexpected issuers, certificate validity period pattern.

Architecture Assessment: In 2–3 sentences, describe what this certificate deployment tells you about the organization's PKI architecture and operational approach. This is not a grade — it is an observation.

---

## Results

- I succcessfully retreived the live TLS certifcate from capitalone.com using openssl, and the TLS handshake completed succuessfully
- 3 certficate blocks populated when obtaining the full certificate chain
- The validity field: Not Before: Jan 9 00:00:00 2026 GMT and Not After: Jan 8 23:59:59 2027 GMT, with approximately 364 days remaining before expiration
- The Subject field shows CN=capitalone.com and O=Capital One Financial Corporation, which indicates the certificate type is OV (Organization Validation)
- The Issuer is CN=DigiCert EV RSA CA G2, which is a known public CA
- The chain terminates at a public root CA named DigiCert Global Root G2
- the SAN field only populates one domain: DNS:capitalone.com
- The SSL Labs results: A+ overall grade, supports TLS version 1.3, Deprecated TLS versions 1.0 and 1.1 are not supported, STS (HTTP Strict Transport Security) is configured, and OCSP stapling is also supported
- Certificate Transparency (crt.sh) results: Approximately 100 certificates issued for the domain, The most recent issuer is CN=DigiCert EV RSA CA G2 and their old issuer that was used frequently CN=Symantec Class 3 EV SSL CA - G3 is no longer active. The domain also uses a paid Public CA Digicert. 


<img width="501" height="574" alt="Enterpries_cert" src="https://github.com/user-attachments/assets/11b85c49-6c03-4a02-ba8e-e638dadaf1a0" />

```

---

## Key Findings
Document the most important observations from the lab.

Examples:

- What you discovered about the certificate, key, or protocol
- How a specific field or extension affected the outcome
- What a validation result indicated
- Any unexpected behavior or results

-
-
-

---

## Explanation
Explain **why the results matter**.

Examples:

- Why a specific field or extension is required
- Why a validation succeeded or failed
- What the result means in a real-world PKI context
- How this connects to the week's learning outcomes

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

Examples:

- Command errors
- Missing files or dependencies
- Verification failures and how you diagnosed them

---

## Artifacts
Enterprise_cert.pem, lab-01-enterprise-certificate.md

---

*CVI PKI Career Pathway — Foundations Phase*
