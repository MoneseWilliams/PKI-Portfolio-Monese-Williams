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
- The subject fields that were included were the Common Name, Organization, Organizational Unit, Country, State, and Locality. Each of these fields communicates the subject’s identity to the CA.
  
- The output from openssl req -text when I inspected the CSR showed the subject fields and the public key algorithm, confirming that it is an RSA key, also presented the signature value confiming the csr is signed
  
-The CSR and the signed certificate differ because the CSR is just a request sent to the CA, while the signed certificate is the final issued certificate after the CA signs it. However, their public keys remain the same, which is why when I used the diff command, there was no output.
  
- The diff output showed nothing when comparing the CSR public key to the signed certificate public key because there was no difference.

## Key Findings

A key finding i've found is that even though the CSR and the signed certificate are different, they both contain the same public key, which confirms that the certificate was generated from the original private key. This was verified by comparing the public keys and observing that there was no difference.

## Explanation
- The private key must never leave the requestor’s machine, even when submitting a CSR to a CA, because it is sensitive and must remain secret. If it is exposed, it can compromise the requestor’s identity and cause the issued certificate to no longer be trusted and be revoked.
  
- A CSR is a certificate request generated from system’s private key that you submit to a CA to request a signed certificate. A signed certificate is the certificate you receive from the CA that contains your public key and proves the CA verified your identity with its signature.
  
- In the real world, self-signing a certificate is not appropriate unless it is in an internal enterprise PKI environment. This is because, for a system to trust a certificate, it must be issued and signed by a trusted root CA so the system can build a trust chain and verify that the certificate is valid. However, in internal environments, certificates can be self-signed because the root CA is created and managed by the organization and is trusted within their systems only.

## Challenges / Troubleshooting

A challengeed I faced during thre lab was interpreting certain OpenSSL outputs. For example, I initially thought that “Certificate request self-signature ok” meant the certificate was trusted, but I later understood that it only confirmed the CSR and signing process worked, not that the certificate was trusted by a system.

## Artifacts
- test_csr.pem, test_cert.pem

## Stretch

For my stretch lab, I added a Subject Alternative Name (SAN) extension to my generated CSR and re-signed the certificate. In week 3, I saw the SAN in the X.509 extension fields. It is required for modern TLS certificates because the Common Name (CN) alone is not sufficient, and multiple domains can be included under the same certificate.
The CN typically shows the main domain name, while the SAN includes all domain names associated with the certificate. A certificate can include multiple SAN entries because they are all related to the same domain. In this lab, the SAN entries included DNS:lab01-san.cvi.internal and DNS:www.lab01-san.cvi.internal.
