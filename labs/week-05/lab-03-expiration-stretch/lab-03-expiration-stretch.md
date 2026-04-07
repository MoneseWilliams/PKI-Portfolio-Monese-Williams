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

## Expired vs Replacement

When comparing test_cert_expired.pem and test_cert_replacement.pem, I was able to see that the validity period for the expired certificate was notBefore=Jan 1 00:00:00 2023 GMT and notAfter=Jan 2 00:00:00 2023 GMT, meaning the certificate had already expired. meanwhile, the replacement certificate showed a validity period of notBefore=Apr 7 12:59:35 2026 GMT and notAfter=Apr 7 12:59:35 2027 GMT, meaning it is valid for one year.
This confirms the replacement workflow, where I generated a new private key, created a new CSR, and issued a new certificate. The main change is the updated validity period and the use of a new private key, which ensures the certificate is secure and usable within the system for a longer period of time.


## Results
- The output I received using the openssl x509 -checkend command showed “certificate will not expire” for 3600 seconds (1 hour), but for 86400 seconds (1 day), it showed “certificate will expire.” This confirms that the certificate will expire within 1 day and that this command can be used to check whether a certificate will expire within a specified amount of time.
  
- the error that had appeared when i ran openssl verify on the expired certificate was " certificate expired" and " verification failed" cinfiming that the certifcate was indeed expired
  
- The exit code behavior of -checkend 0 vs -checkend 86400 is that -checkend 0 checks whether the certificate is still valid at that current moment, while -checkend 86400 checks whether it will still be valid 1 day from now. If the certificate will not expire within that time, the command exits with code 0, but if it will expire within that time, it exits with code 1. I could use this in a script to automate certificate expiration alerts by checking different time windows, such as 90, 60, and 30 days before expiration, and sending reminders when the certificate is getting close to expiring assiting with the 90 60 30 day rule. 
  
- For the full replacement workflow, I generated a new private key, used the new private key to create a CSR, and then used the CSR to self-sign and issue a new replacement certificate with an updated validity period. Lastly, I verified that the new certificate is valid.

## Key Findings

Through this lab, I learned how certificate expiration can impact systems and why it is one of the main causes of outages in enterprise PKI environments. I was able to generate a certificate with a short validity period and observe how quickly it can expire and cause validation failures. I also learned how to use the openssl x509 -checkend command to determine whether a certificate will expire within a specific amount of time, which is useful for monitoring and alerting before expiration occurs.

## Explanation
- The difference between certificate renewal and certificate replacement is that certificate replacement involves generating a new private key and issuing a new certificate, which is required when a certificate has expired. Certificate renewal is used when a certificate is still valid but close to expiring and genrally keeping the same private key.
  
- You would choose a replacement certificate over renewal if the certificate private key was suspected to be compromised or stolen.
  
- Certificate expiration still causes enterprise outages despite being entirely predictable because there is no inventory of all the certificates within the enterprise, no alerts reminding when certificates need to be renewed before reaching expiration, and even because the person responsible for renewing the certificates becomes unavailable right before expiration.

## Challenges / Troubleshooting

One challenge I faced during this lab was trying to create an expired certificate using OpenSSL. At first, I used the -startdate and -enddate options, but I kept getting an error saying x509: Extra option, and the certificate was not created. Because the certificate was never created, I kept getting “No such file or directory” errors when I tried to inspect or verify it, which confused me at first. After checking the OpenSSL help output, I realized that I was using the wrong options. I then switched to using -not_before and -not_after, which allowed me to successfully create the expired certificate and continue the lab.

## Artifacts
- test_cert_short.pem, test_cert_expired.pem, test_cert_replacement.pem
