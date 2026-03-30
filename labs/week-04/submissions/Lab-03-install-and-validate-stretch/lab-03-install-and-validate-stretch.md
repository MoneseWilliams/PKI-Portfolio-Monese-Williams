# Lab 03 — Install a Certificate and Validate Trust (Stretch)

## Overview
Briefly describe what this lab was about in your own words. What PKI concept or system behavior were you investigating?

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (openssl version):

## Steps Performed
1.First, I created an artifact directory to place my artifacts in during the lab. I then used OpenSSL to generate a private key and a self-signed root CA, and verified that the generated certificate was valid using the OpenSSL x509 command.

2.I then verified that the Root CA was made successfully by inspecting the certificate and confirming that the Subject and Issuer were both CN=CVI-Lab-Root-CA, O=CyberVisionaries Institute, C=US, which confirms it is self-signed. The validity period was Not Before: Mar 30 16:45:18 2026 GMT and Not After: Apr 29 16:45:18 2026 GMT, meaning it will expire in April 2026.

3.Next, I used the sudo command to add a root certificate to my macOS trust store and mark it as trusted. I was prompted to enter my system password to finalize the change.

4.Once my generated root CA was added to my macOS trust store, I then generated a private key so it could be used to sign the certificate, as well as a CSR to send to my generated root CA requesting that a certificate be issued and signed.

5. To verify the generated certificate, I used the OpenSSL verify command and received an output of test-signed.crt: OK, which confirmed that the signed certificate was issued by a trusted root CA within the macOS trust store.
   
6. Lastly, I removed the trusted root CA that I created from the macOS trust store and verified it was fully deleted by using the security find-certificate command, which returned no output, confirming it was deleted. I did make an attempt at first to rerun the OpenSSL verify command to confirm it was deleted, but it still returned test-signed.crt: OK because the command was using the -CAfile option and manually pointing to the root CA file rather than checking the macOS trust store.

## Results
- What did the certificate output show when you verified test-root-ca.crt?
- What did the verify output return after signing — before and after cleanup?
- What confirmed the trust chain was established?

Test Root Ca

<img width="463" height="317" alt="test root ca cert" src="https://github.com/user-attachments/assets/0246f4ce-7866-493f-9daf-93a33d9754e9" />

Test Signed Certificate 

<img width="494" height="303" alt="test signed cert" src="https://github.com/user-attachments/assets/6043b5fd-4db8-4fd9-8a94-b0e81584905a" />

Test Signed CSR 

<img width="508" height="244" alt="test signed csr" src="https://github.com/user-attachments/assets/9798c5fb-1343-4715-8ba0-dac174d3f625" />



## Key Findings

## Explanation
- What made the test root CA self-signed? How did you identify that in the output?
- What changed on your system after you installed the root CA?
- In an enterprise environment, who controls what root CAs are installed on employee machines?
- Why is it a security concern if an attacker can install a root CA on a device?

## Challenges / Troubleshooting

## Artifacts
- test-root-ca.crt, test-signed.crt, test-signed.csr
