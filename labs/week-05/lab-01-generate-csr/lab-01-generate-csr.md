# Lab 01 — Generate a CSR

## Overview
Briefly describe what this lab was about in your own words. What PKI concept or workflow were you investigating?

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (openssl version):

## Steps Performed
Summarize the key steps you performed. Do not copy the lab instructions — describe what you actually did.
1.
2.
3.
4.
5.

## Results
- What Subject fields did you include in your CSR, and what does each field communicate to a CA?
- What output did `openssl req -text` show when you inspected your CSR?
- How did the CSR differ from the signed certificate when you compared them?
- What did the diff output show when you compared the public key in the CSR vs the signed cert?

## Key Findings

## Explanation
- Why must the private key never leave the requestor's machine — even when submitting a CSR to a CA?
- What is the difference between a CSR and a signed certificate?
- In what real-world scenario would self-signing be appropriate vs submitting to a trusted CA?

## Challenges / Troubleshooting

## Artifacts
- test_csr.pem, test_cert.pem
