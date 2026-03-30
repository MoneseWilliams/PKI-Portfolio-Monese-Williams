# Lab 03 — Install a Certificate and Validate Trust (Stretch)

## Overview
Briefly describe what this lab was about in your own words. What PKI concept or system behavior were you investigating?

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (openssl version):

## Steps Performed
1. First, I created an artifact directory to place my artifacts in during the lab. I then used OpenSSL to generate a private key and a self-signed root CA, and verified that the generated certificate was valid using the OpenSSL x509 command.

2. I then verified that the Root CA was made successfully by inspecting the certificate and confirming that the Subject and Issuer were both CN=CVI-Lab-Root-CA, O=CyberVisionaries Institute, C=US, which confirms it is self-signed. The validity period was Not Before: Mar 30 16:45:18 2026 GMT and Not After: Apr 29 16:45:18 2026 GMT, meaning it will expire in April 2026.

3. Next, I used the sudo command to add a root certificate to my macOS trust store and mark it as trusted. I was prompted to enter my system password to finalize the change.

4. Once my generated root CA was added to my macOS trust store, I then generated a private key so it could be used to sign the certificate, as well as a CSR to send to my generated root CA requesting that a certificate be issued and signed.

5. To verify the generated certificate, I used the OpenSSL verify command and received an output of test-signed.crt: OK, which confirmed that the signed certificate was issued by a trusted root CA within the macOS trust store.
   
6. Lastly, I removed the trusted root CA that I created from the macOS trust store and verified it was fully deleted by using the security find-certificate command, which returned no output, confirming it was deleted. I did make an attempt at first to rerun the OpenSSL verify command to confirm it was deleted, but it still returned test-signed.crt: OK because the command was using the -CAfile option and manually pointing to the root CA file rather than checking the macOS trust store.

## Results
- When I verified test-root-ca.crt, the certificate output showed that the Subject and Issuer were the same, confirming that it is a self-signed root CA. It also displayed the validity period, showing when the certificate becomes valid and when it expires. This verified that the root CA was created successfully.
  
- After signing the certificate, the verify output returned test-signed.crt: OK, confirming that the certificate was successfully signed by the root CA and was valid. After cleanup, when I removed the root CA from the macOS trust store and ran the verify command again, it still returned test-signed.crt: OK because the command was using the -CAfile option as a reference instead of the macOS trust store.
  
- I was able to confirm the trust chain was established when I used the OpenSSL verify command and received the output test-signed.crt: OK. This showed that the certificate successfully chained back to my root CA and was trusted. 

Test Root Ca

<img width="463" height="317" alt="test root ca cert" src="https://github.com/user-attachments/assets/0246f4ce-7866-493f-9daf-93a33d9754e9" />

Test Signed Certificate 

<img width="494" height="303" alt="test signed cert" src="https://github.com/user-attachments/assets/6043b5fd-4db8-4fd9-8a94-b0e81584905a" />

Test Signed CSR 

<img width="508" height="244" alt="test signed csr" src="https://github.com/user-attachments/assets/9798c5fb-1343-4715-8ba0-dac174d3f625" />


## Key Findings

## Explanation
-The test root CA is self-signed because the Subject and Issuer fields both matched. I identified this in the output when I viewed the certificate using OpenSSL, which confirmed that the certificate signed itself.

- After installing the root CA, it was added to my macOS trust store, meaning my system would now trust any certificates that are signed by that root CA without showing warnings.
  
- In an enterprise environment, system administrators or the IT/security team control what root CAs are installed on employee machines, usually through tools like Group Policy or MDM.
  
- It is a security concern because an attacker could use that root CA to issue fake certificates, making malicious websites or connections appear trusted. This could allow them to perform man-in-the-middle attacks without the user knowing.

## Challenges / Troubleshooting
One challenge I faced during this lab was understanding the difference between verifying a certificate and inspecting a certificate. At first, I thought that the output “Certificate request self-signature ok” confirmed the trust chain, but I later realized that it only confirmed the CSR and signing process worked. I learned that the OpenSSL verify command is what actually confirms whether a certificate is trusted.

Another challenge I encountered was understanding why the certificate still returned OK after I removed the root CA from the macOS trust store. I realized that this was because I used the -CAfile option, which manually points OpenSSL to a specific root CA file instead of using the system trust store.

## Artifacts
- test-root-ca.crt, test-signed.crt, test-signed.csr
