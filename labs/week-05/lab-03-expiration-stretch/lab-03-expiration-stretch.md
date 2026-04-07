# Lab 03 — Simulate a Certificate Expiration Scenario (Stretch)

## Overview
In this lab, I will be better understanding the certificate expiration process and why it is one of the main causes of systems breaking within a PKI enterprise environment. This lab will help me gain the skills of a real engineer to implement the workflow of how to detect and renew a certificate after it has expired.

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version): OpenSSL 3.6.0

## Steps Performed

1. First, I created a directory on my system to store all my artifacts for this lab. I then generated my own private key and used it to create a CSR. I issued a certificate using the CSR and gave it a short validity window. I then verified that it was created properly by inspecting the certificate and reviewing the validity period, which showed notBefore=Apr 6 20:49:13 2026 GMT and notAfter=Apr 7 20:49:13 2026 GMT, showing the certificate is only valid for 1 day. If any system attempts to trust this certificate after that time, it will fail validation because the certificate has expired.

2. Next, I used the OpenSSL x509 -checkend command to check whether a certificate will expire within a specified amount of time, which is very useful in a real PKI environment. I first checked using 1 hour and received the output “certificate will not expire,” but when I checked again for 1 day, it returned “certificate will expire.” I would typically use this command in a monitoring script to alert on certificates expiring within 30 days by checking the certificate’s expiration window in advance to provide a heads-up.

3. Next, I created another certificate using OpenSSL with a validity period that has already passed, making it an expired certificate. This simulates a real world PKI scenario where certificates can expire and cause systems to fail if not renewed in time. This helped me understand what to expect when working with expired certificates and how to identify them. When I verified the expired certificate using OpenSSL, it returned an error "verification failed", indicating that the certificate has expired. This directly relates to what a browser or server would display to an end user, such as a warning that the connection is not secure or that the certificate is no longer valid.

4. Lastly, since the certificate is expired, the proper step is to replace it with a new certificate and a new private key for security purposes. I generated a new private key and created a new CSR, then used the new CSR to issue a new certificate. I verified that the new certificate was valid and observed an output stating “certificate will not expire,” along with the validity period showing notBefore=Apr 7 12:59:35 2026 GMT and notAfter=Apr 7 12:59:35 2027 GMT.

5. 
6.
7.
8.

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
