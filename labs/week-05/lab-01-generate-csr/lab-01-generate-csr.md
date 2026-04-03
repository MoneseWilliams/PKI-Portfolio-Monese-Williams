# Lab 01 — Generate a CSR

## Overview
In this lab, I will be understanding the certificate issuance workflow by breaking down the main five steps and implementing them on my operating system. I will be mirroring the process of generating a private key within my system, creating a CSR to submit to a generated root CA, validating and signing the certificate, and then issuing the certificate with verification that it worked with no errors, as simulated in a real PKI environment.

## Environment
- Operating System: macOS
- Terminal Used: Terminal
- OpenSSL Version (openssl version):OpenSSL 3.6.0

## Steps Performed

1. First, i created a directory to store my artifacts in for this lab, i thne proceeded to do the first step of the certificate issuance process by generating a RSA private key with a size of 2048 bits, and verifying the private key was created succuessfully. 
2. Next, I created a certificate signing request (CSR) using the generated RSA private key and inspected the subject fields of the request that will be included in the issued certificate. Below is a table of the subject fields that are included in an X.509 certificate.

| Field               | Abbreviation | Value in Your CSR |
|--------------------|-------------|-------------------|
| Common Name        | CN          |lab01.cvi.internal |
| Organization       | O           |CyberVisionaries Institute|
| Organizational Unit| OU          |PKI Career Pathway |
| Country            | C           |US                 |
| State              | ST          |California         |
| Locality           | L           |San Francisco      |


3. Then, I went and self-signed the certificate to simulate a CA, since in a real PKI environment you would normally submit the CSR to a CA to verify, sign, and issue the certificate once validation of the subject’s identity is successful. After signing, I received the output “Certificate request self-signature ok,” which verified that the certificate was signed. I then inspected the newly issued certificate to confirm that the subject fields from the CSR matched the certificate, which they did.
4. Lastly, I compared the certificate to the csr by verifying whether the public keys matched. I extracted the public key from the certificate and the public key from the private key used to generate the CSR, then used the diff command, which returned no output, confirming that the public keys are identical.
  

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
