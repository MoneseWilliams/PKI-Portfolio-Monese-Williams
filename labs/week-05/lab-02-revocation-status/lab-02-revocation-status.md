# Lab 02 — Check Certificate Revocation Status with OCSP

## Overview
Briefly describe what this lab was about in your own words. What PKI concept or system behavior were you investigating?

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version):OpenSSL 3.6.0
- Website Used for Certificate Retrieval: 

## Steps Performed

1. First, I created a directory within my system to store all my artifacts for this lab. Then, I connected to twitch.tv and retrieved its live certificate to work with. I verified that it was saved correctly and was able to confirm that it was issued to twitch.tv and issued by GlobalSign Atlas R3 DV TLS CA 2025 Q2.

2. Next, I wanted to perform an OCSP query, but in order to do so, I had to gather both the leaf certificate and the issuer’s certificate. I used the echo command with OpenSSL to retrieve the full certificate chain from the server and saved the issuer certificate to its own file named issuer_cert.pem, then verified it was saved correctly. I was also able to observe that the subject and issuer of the issuer certificate are different from the leaf certificate, since the issuer certificate is a CA and is responsible for signing the leaf certificate.

3. Next, I obtained the revocation information within the certificate. I inspected the leaf certificate’s extensions and found the OCSP URL: URI:http://ocsp.globalsign.com/ca/gsatlasr3dvtlsca2025q2. I then used OpenSSL to locate the CRL Distribution Point and obtained the URL: http://crl.globalsign.com/ca/gsatlasr3dvtlsca2025q2.crl.

4. Lastly, I queried the OCSP responder by extracting the OCSP URL from the leaf certificate. After officially querying the responder, I was able to see leaf_cert.pem: good, meaning the certificate is valid and has not been revoked. I also observed the timestamps This Update: Apr  6 06:00:00 2026 GMT and 	Next Update: Apr  6 18:00:00 2026 GMT, which indicate when the OCSP response was last updated and when it will be updated again. The query also requires the issuer certificate in addition to the leaf certificate in order to properly validate the status.

## Diffrences Between OCSP and CRL

Based on what I observed in this lab, the purpose of OCSP is to provide real-time status updates for certificates, while a CRL lists all revoked certificates published by the CA on a schedule. OCSP is better for high-traffic systems because it allows for live queries with more up-to-date results, whereas CRLs may not reflect changes immediately due to their scheduled updates

Even though OCSP is better for high traffic systems, if the responder is unavailable, most systems will perform an OCSP soft fail and allow the certificate anyway. This is because it is often considered worse to deny a legitimate user access than to allow access and discover later that the certificate has been revoked.
A hard fail, on the other hand, will block access to the certificate if the responder is unavailable.

An organization might choose to run its own OCSP responder instead of relying on a public CA’s responder because this allows the organization to handle all certificate checks internally without the need for external services. This helps avoid issues if the CA’s responder becomes unavailable.
  
## Results
- What OCSP URL did you find in the certificate's Authority Information Access extension?
- What was the OCSP response status (`good`, `revoked`, or `unknown`) and what does it mean?
- What were the `This Update` and `Next Update` values in the OCSP response, and what do they indicate?
- Where was the CRL Distribution Point located in the certificate?

## Key Findings

## Explanation
- What is the difference between OCSP and CRL as revocation checking methods?
- Why does an OCSP query require both the leaf certificate and the issuer certificate?
- In what scenario would a certificate show `unknown` status from an OCSP responder?

## Challenges / Troubleshooting

## Artifacts
- leaf_cert.pem, issuer_cert.pem, ocsp_response.txt
