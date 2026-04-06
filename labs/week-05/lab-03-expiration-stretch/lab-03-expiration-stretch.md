# Lab 03 — Simulate a Certificate Expiration Scenario (Stretch)

## Overview
Briefly describe what this lab was about in your own words. What PKI concept or operational scenario were you investigating?

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version): OpenSSL 3.6.0

## Steps Performed
Summarize the key steps you performed. Do not copy the lab instructions — describe what you actually did.
1.
2.
3.
4.
5.

## Results
- What output did `openssl x509 -checkend` produce for the short-lived certificate?
- What error appeared when you ran `openssl verify` on the expired certificate?
- What is the exit code behavior of `-checkend 0` vs `-checkend 86400`, and how would you use this in a script?
- Walk through the replacement workflow steps you performed: new key → new CSR → new cert.

## Key Findings

## Explanation
- What is the difference between certificate renewal and certificate replacement?
- When would you choose replacement over renewal, even if the certificate hasn't expired?
- Why does certificate expiration still cause enterprise outages despite being entirely predictable?

## Challenges / Troubleshooting

## Artifacts
- test_cert_short.pem, test_cert_expired.pem, test_cert_replacement.pem
