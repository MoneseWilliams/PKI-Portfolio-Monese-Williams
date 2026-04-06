# Lab 02 — Check Certificate Revocation Status with OCSP

## Overview
In this lab, I will be retrieving a live certificate from a website, inspecting it, and determining whether the certificate has been revoked or not by using the Certificate Revocation List (CRL) and an OCSP responder. This ties heavily into real world PKI environments due to the importance of checking certificate status.

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version):OpenSSL 3.6.0
- Website Used for Certificate Retrieval: Twitch.tv

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
- The OCSP URL I found in the certificate's Authority Information Access extension is http://ocsp.globalsign.com/ca/gsatlasr3dvtlsca2025q2
  
- The OCSP response status that populated was Good which means the Certificate is valid and has not been revoked
  
- In the OCSP response the timestamps This Update: Apr  6 06:00:00 2026 GMT and Next Update: Apr  6 18:00:00 2026 GMT and indicate when the OSCP Responder last updated and when it will be updated again.
  
- The CRL Distribution Point was located in the extensions part of the X.509 certificate

## Key Findings

A key finding i have is the difference between OCSP and CRL. OCSP provides real-time certificate status, while CRLs are published on a schedule and may not always be up to date. I also learned that most systems use a soft fail if the OCSP responder is unavailable, meaning the certificate may still be trusted even if its status cannot be checked.

## Explanation
- The difference between OCSP and CRL as revocation checking methods is that OCSP is a real-time responder that provides up to date status updates on a certificate, whereas a CRL is a list of certificate serial numbers that have been revoked and is only updated on a schedule. This can cause a delay or lag in revocation status updates when using a CRL.
  
- An OCSP query requires both the leaf certificate and the issuer certificate because it needs to identify which CA issued the certificate in order to properly validate its status.
  
- A certificate wuld only show unknown status from an OCSP responder if The OCSP responder does not have status information for this certificate

## Challenges / Troubleshooting

I also had some difficulty understanding why both the leaf certificate and issuer certificate were required for the OCSP query. After working through the lab, I realized that the issuer certificate is necessary to identify which CA issued the certificate so the responder can properly check its status.

## Artifacts
- leaf_cert.pem, issuer_cert.pem, ocsp_response.txt
