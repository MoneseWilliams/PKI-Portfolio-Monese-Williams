# Lab 01 — Convert Certificate Formats

## Overview
n this lab, I will be understanding one of the most common issues PKI engineers face but may not realize until they come across it. This lab will help me better understand PEM, DER, and PFX certificate formats and how to convert them using OpenSSL. This contributes to the concept in PKI that certificates do not change when converting formats, only the encoding changes.

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version):OpenSSL 3.6.0

## Steps Performed

1. To begin this lab, I determined I first wanted to obtain a certificate in PEM format. I chose google.com as my website and used OpenSSL to retrieve the leaf certificate from the site. I then saved it as a file named leaf_cert.pem.
   
2. I then created a directory to store all of my artifacts for each file conversion I chose to perform. After that, I moved my leaf certificate PEM file into that directory for better organization.

3. The BEGIN and END lines of the leaf certificate confirm that the encoding is in PEM format. I then opened the file to observe the X.509 certificate.
   
4. Next, I converted the PEM file into DER format using OpenSSL and named it leaf_cert.der. I then opened the DER file in a readable format to confirm it was valid. I observed that the file still contained the same information as the PEM format, confirming that format conversion does not change the actual certificate, only the encoding. I also opened the DER file and noticed it was full of random characters and spaces, verifying it is in DER format.

5. After successfully converting the certificate from PEM format to DER format, I decided to convert the file back into PEM format, naming the new reconverted file leaf_cert_restored.pem, and compared it to the original PEM file leaf_cert.pem. When running the diff command, there was no output between the two files, verifying that the conversion back to PEM was successful.

6. Next i will be working with certifcate format PFX. PFX format consists a bundle of the certificate and private key. I generated my own private key, used it to create a new certifcate and sign it. Then bundled the private key and certificate together to create a PFX file with my own password.

7. Lastly, I verified the PFX file was valid by opening the file and entering my password. Inside the file, it revealed that both the certificate and the private key are encrypted within the file.

## Results
- When it came to the PEM file vs the DER file, the PEM file, when opened, shows a certificate block with random characters between -----BEGIN CERTIFICATE----- and -----END CERTIFICATE-----, while the DER file only shows random characters in binary format when opened. However, when both were converted into a readable text format, they contained the same exact information, including the leaf certificate.
  
- When opening the DER file in a text editor, the contents appeared as unreadable characters and symbols because the file is in a binary format.
  
- After converting the PEM file to DER and back to PEM, I used the diff command to compare both files. There was no output, which verified that there were no differences between them and that the conversion was successful.
  
- When I verified the PFX file, It showed that the file contains both the certificate and the private key, and that they are encrypted and protected using a password.

![Leaf Cert](../../assets/screenshots/week-04/leaf_cert.png)

## Key Findings

- Through this lab, I learned that certificate formats such as PEM, DER, and PFX all contain the same certificate data, but differ in how that data is encoded. PEM files are Base64-encoded and readable, while DER files are binary and not human-readable. PFX files bundle both the certificate and private key together and are protected with a password.


## Explanation
-  A PFX file requires a password because it contains a private key that must remain protected from unauthorized access.
  
- In real-world scenarios, the certificate format depends on the system being used. Linux web servers expect PEM format, which is readable text. Windows machines expect PFX (PKCS#12), which bundles the certificate and private key together. Java applications expect DER format, which is a binary format. Each system requires a specific format, and if the wrong format is used, the certificate will be rejected even if it is valid.
  
- It is important to never commit private key files to GitHub because a private key is meant to stay secret. If it is exposed, anyone can use it to impersonate you, access secure systems, or decrypt sensitive data.

## Challenges / Troubleshooting

-One challenge I faced during this lab was navigating file paths and directories in the terminal. At times, commands failed because the directory I was trying to access did not exist or I was not in the correct location. I resolved this by using commands like pwd and ls to confirm my current directory and ensure the correct file paths were being used. Another challenge was understanding why certain files, like DER and PFX, appeared unreadable when opened with commands like cat or nano. I learned that these formats are binary and not meant to be read directly, and instead require tools like OpenSSL to properly view their contents.

## Artifacts
- leaf_cert.pem, leaf_cert.der, leaf_cert_restored.pem, test_cert.pem, test_bundle.pfx
