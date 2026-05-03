# Lab — Enterprise Certificate Analysis

## Overview
In this lab, I will be retrieving a live certificate from a well-known enterprise organization and performing a complete certificate analysis on the TLS certificate and its full deployment. I will be investigating where TLS terminates within the environment and documenting my findings.

---

## Steps Performed

1. First, I will be retrieving the live certificate from my target enterprise, Capital One Bank, using openssl s_client. I then viewed the live certificate to confirm it populated successfully and decided to pull up the full certificate chain for Capital One Bank. The full certificate TLS handshake completed successfully and populated three certificate blocks.

2. Next, I parsed the live certificate and inspected it, recording the validity field: Not Before: Jan 9 00:00:00 2026 GMT and Not After: Jan 8 23:59:59 2027 GMT, with approximately 364 days remaining before expiration. The Subject field shows CN=capitalone.com and O=Capital One Financial Corporation, and the certificate policy number present (2.23.140.1.1) indicates the certificate type is EV (Extended Validation) due to the organization details it includes. In the SAN field, it also only shows one entry, DNS:capitalone.com, with no wildcard entries. This suggests the certificate architecture is only valid for the domain capitalone.com. The Issuer is CN=DigiCert EV RSA CA G2, which is a known public CA, DigiCert.

3. Then, I went ahead and analyzed the full certificate chain. There are a total of three certificates in the chain. The leaf certificate is present, being the first certificate in the chain. The intermediate CA is also present, with the Subject being CN=DigiCert EV RSA CA G2. The chain then terminates at a public root CA named DigiCert Global Root G2, confirming that this chain is complete.

4. Next, I determined where TLS terminates and whether there were any CDN or load balancer indicators using OpenSSL. The issuer does not suggest that this is a CDN-managed certificate. It is issued by DigiCert, which is a known public CA, but there are no indicators pointing to Cloudflare or Fastly. The subject field outputs O=Capital One Financial Corporation, CN=capitalone.com, which is the company name, and the SAN field populates one entry, DNS:capitalone.com, with no wildcards. Neither are CDN indicators, but both are load balancer indicators. The server output also includes “BigIP,” which is associated with F5 load balancers.


5. Next, I decided to run an SSL analysis using https://www.ssllabs.com/ssltest/ for a complete TLS configuration report. The SSL Labs overall grade is an A+, meaning it has excellent security with strong configurations and supports TLS version 1.3. Deprecated TLS versions 1.0 and 1.1 are not supported under the Protocols tab. HSTS (HTTP Strict Transport Security) is configured, and OCSP stapling is also supported.

6. Lastly, I checked the certificate transparency logs to understand how long the organization has been using this CA and to see if there are any unexpected issuers. I used the site https://crt.sh, and after searching my chosen enterprise hostname, www.capitalone.com, I saw that there are approximately 100 certificates that have been issued for this domain. Recent issuers, such as CN=DigiCert EV RSA CA G2, have been consistent for this domain, but different CAs have also been used, including DigiCert Global G3 TLS ECC SHA384 2020 CA1, CN=DigiCert SHA2 Extended Validation Server CA, and Symantec Trust Network, CN=Symantec Class 3 EV SSL CA - G3. There are also some older or unfamiliar issuers in the CT logs, such as CN=Symantec Class 3 EV SSL CA - G3, which could indicate that this certificate authority is no longer in active use. The approximate validity period of the most recent certificates is around 1 year, since DigiCert is a paid CA and most paid certificates follow a 1 year validity pattern.

---

## Certificate Summary:

Issuer, validity window, certificate type (DV/OV/EV), SAN count, wildcard usage.

For this enterprise live certificate, the issuer is CN=DigiCert EV RSA CA G2, which is a known public CA, with a validity window of Not Before: Jan 9 00:00:00 2026 GMT and Not After: Jan 8 23:59:59 2027 GMT, with approximately 364 days remaining before expiration. So not only did the TLS handshake complete successfully, but the certificate is also still active. The certificate type is EV (Extended Validation), indicated by the organization details in the subject field (O=Capital One Financial Corporation) and the certificate policy ID. Within the SAN field, only one entry is populated, DNS:capitalone.com, with no wildcards, meaning this certificate is only issued to this domain.

---

## Chain Analysis:

During my chain analysis, 3 certificates populated within the chain: the live certificate matching the hostname CN=capitalone.com, with an issuer named CN=DigiCert EV RSA CA G2. The intermediate CA matches what’s populated in the leaf certificate’s subject field, CN=DigiCert EV RSA CA G2, meaning the correct intermediate CA has populated. The same applies for the intermediate CA issuer, CN=DigiCert Global Root G2, which matches the root CA certificate block subject field. The root CA subject and issuer fields both match, which also confirms this is a valid root CA for this chain, verifying the completion of this chain.

---

## Termination Analysis:

During my analysis, I was able to determine that the TLS certificate appears to terminate at the load balancer because there are no CDN indicators within the subject or SAN field. For example, the subject field presents the company’s name (O=Capital One Financial Corporation, CN=capitalone.com) and not a third party, and the SAN entry (DNS:capitalone.com) shows one domain with no wildcards. The certificate was also issued directly to the company, confirming that the company has control of TLS. The server output also includes “BigIP,” which is associated with F5 load balancers, which indicates that TLS most likely terminates at the load balancer.

---

## TLS Configuration: 

For the TLS configuration, I ran an SSL Labs analysis using https://www.ssllabs.com/ssltest/. The SSL Labs overall grade for the capitalone.com certificate is an A+, meaning it has excellent security with strong configurations,The server supports both TLS 1.3 and TLS 1.2, while deprecated versions such as TLS 1.0 and TLS 1.1 are not supported as shown under the Protocols tab. HSTS (HTTP Strict Transport Security) is configured, and OCSP stapling is also supported. 


---

## CT Log Analysis: 

CA consistency, any unexpected issuers, certificate validity period pattern. 

For the CT Log Analysis, I used the site https://crt.sh. I searched my chosen enterprise hostname, www.capitalone.com, and was able to determine that there are approximately 100 certificates that have been issued for this domain. Recent issuers, such as CN=DigiCert EV RSA CA G2, were used consistently for this domain. Different CAs have also been used, including DigiCert Global G3 TLS ECC SHA384 2020 CA1, CN=DigiCert SHA2 Extended Validation Server CA, and Symantec Trust Network, CN=Symantec Class 3 EV SSL CA - G3.

There are also some older or unfamiliar issuers in the CT logs, such as CN=Symantec Class 3 EV SSL CA - G3, which could indicate that this certificate authority is no longer in active use. The approximate validity period of the most recent certificates is around 1 year, which aligns with current industry standards for publicly trusted TLS certificates.

---

## Architecture Assessment:

This certificate deployment indicates that the certificate type is an extended validation (EV), meaning the organization’s identity is strictly verified, which enhances user trust. The TLS terminates at the F5 load balancer based on the server output showing "BigIP" in openssl.

---

## Results

- I succcessfully retreived the live TLS certifcate from capitalone.com using openssl, and the TLS handshake completed succuessfully
- 3 certficate blocks populated when obtaining the full certificate chain
- The validity field: Not Before: Jan 9 00:00:00 2026 GMT and Not After: Jan 8 23:59:59 2027 GMT, with approximately 364 days remaining before expiration
- The Subject field shows CN=capitalone.com and O=Capital One Financial Corporation and the policy id (2.23.140.1.1) indicates the certificate type is EV (Extended Validation)
- The Issuer is CN=DigiCert EV RSA CA G2, which is a known public CA
- The chain terminates at a public root CA named DigiCert Global Root G2
- the SAN field only populates one domain: DNS:capitalone.com
- The SSL Labs results: A+ overall grade, supports TLS version 1.3, Deprecated TLS versions 1.0 and 1.1 are not supported, STS (HTTP Strict Transport Security) is configured, and OCSP stapling is also supported
- Certificate Transparency (crt.sh) results: Approximately 100 certificates issued for the domain, The most recent issuer is CN=DigiCert EV RSA CA G2 and their old issuer that was used frequently CN=Symantec Class 3 EV SSL CA - G3 is no longer active. The domain also uses a paid Public CA Digicert. 


<img width="501" height="574" alt="Enterpries_cert" src="https://github.com/user-attachments/assets/11b85c49-6c03-4a02-ba8e-e638dadaf1a0" />

---

## Key Findings

Some key findings I was able to determine were that this certificate type is Extended Validation, mainly due to the certificate policy ID number 2.23.140.1.1. The TLS configuration is secure based on the analysis, and the certificate is not expired, with the issuer being a trusted public CA. TLS appears to terminate at the load balancer, which is a strong finding based on the server output including “BIGIP,” which is associated with F5 load balancers.

---

## Explanation

These results matter because they show that the certificate is working correctly and is secure. The certificate is valid, not expired, and issued by a trusted CA, which means users can trust the connection. The strong TLS configuration also helps protect data from being intercepted by attackers, showing that the organization is using secure methods to handle this certificate.

---

## Challenges / Troubleshooting

A challenge I faced was determining whether the certificate type was EV or OV because organization information was present in the subject field, and both OV and EV certificates include organization details. At first, I thought this alone would indicate OV, which caused confusion. I was able to figure it out by identifying that EV certificates include a specific certificate policy ID, and since the policy number 2.23.140.1.1 was present, it confirmed that the certificate is EV.

Another challenge I faced was determining how to explain the architecture assessment because I was unsure whether it should be explained in a technical way or not. I also struggled with explaining everything I found during the deployment analysis because I did not fully understand what some of the findings meant at first. However, after doing my own research, I realized that I mainly needed to explain how the certificate is managed and what findings support that.

---

## Artifacts
Enterprise_cert.pem, lab-01-enterprise-certificate.md

---

*CVI PKI Career Pathway — Foundations Phase*
