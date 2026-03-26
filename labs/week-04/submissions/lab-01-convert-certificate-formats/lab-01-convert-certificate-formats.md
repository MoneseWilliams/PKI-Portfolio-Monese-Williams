# Lab 01 — Convert Certificate Formats

## Overview
n this lab, I will be understanding one of the most common issues PKI engineers face but may not realize until they come across it. This lab will help me better understand PEM, DER, and PFX certificate formats and how to convert them using OpenSSL. This contributes to the concept in PKI that certificates do not change when converting formats, only the encoding changes.

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version):OpenSSL 3.6.0

## Steps Performed

1. To begin this lab, I determined I first wanted to obtain a certificate in PEM format. I chose google.com as my website and used OpenSSL to retrieve the leaf certificate from the site. I then saved it as a file named leaf_cert.pem.
2.I then created a directory to store all of my artifacts for each file conversion I chose to perform. After that, I moved my leaf certificate PEM file into that directory for better organization.
3. The BEGIN and END lines of the leaf certificate confirm that the encoding is in PEM format. I then opened the file to observe the X.509 certificate.
4 Next, I converted the PEM file into DER format using OpenSSL and named it leaf_cer.der. I then opened the DER file in a readable format to confirm it was valid. I observed that the file still contained the same information as the PEM format, confirming that format conversion does not change the actual certificate, only the encoding. I also opened the DER file and noticed it was fill of random characters and spaces verifying it is in the DER format.
5.After succuesfully converting the certifacte from PEM format to DER format. I decided to convert the file back into PEM format, name the new reconverted file leaf_cert_restored.pem and compare it to the original PEM file leaf_cert.pem 

## Results
- What did the PEM file look like compared to the DER file?
- What happened when you opened the .der file in a text editor?
- What did the diff output show after converting PEM → DER → PEM?
- What information was displayed when you verified the PFX?

![Description of your screenshot](../../../assets/screenshots/your-filename.png)

## Key Findings

## Explanation
- Why does a PFX require a password?
- In what real-world scenario would you choose PEM vs DER vs PFX?
- Why is it important never to commit private key files to GitHub?

## Challenges / Troubleshooting

## Artifacts
- leaf_cert.pem, leaf_cert.der, leaf_cert_restored.pem, test_cert.pem, test_bundle.pfx
