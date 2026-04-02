# Lab 02 — Check Certificate Revocation Status with OCSP

## Overview
Briefly describe what this lab was about in your own words. What PKI concept or system behavior were you investigating?

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (openssl version):
- Website Used for Certificate Retrieval:

## Steps Performed
Summarize the key steps you performed. Do not copy the lab instructions — describe what you actually did.
1.
2.
3.
4.
5.

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
