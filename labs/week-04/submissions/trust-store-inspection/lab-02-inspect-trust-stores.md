# Lab 02 — Inspect Your Trust Store

## Overview
This lab will boost my understanding of the purpose of trust stores, where they are located on an operating system, and how they are used in a real PKI environment. I will be investigating certificate authorities and verifying a specific root CA.

## Environment
- Operating System:macOS
- Terminal Used:Terminal
- OpenSSL Version (openssl version\):Openssl 3.6.0

## Steps Performed
1.First, I created a new artifact directory named "trust-store-inspection" to save all my artifacts during the lab. I then used a command to list all the root CAs and recorded the amount in my trust store, which for my operating system, macOS, is located in Keychain Access.

2.Next, I used OpenSSL to extract the first root CA located in my trust store so I could inspect the certificate. I observed the Subject to be CN=Go Daddy Root Certificate Authority - G2, and the Issuer to be the same. The Validity period was Not Before: Sep 1 00:00:00 2009 GMT and Not After: Dec 31 23:59:59 2037 GMT, verifying that the Root CA is self-signed and expires in 2037.

3.I then proceeded to do a visual inspection of my trust store using Keychain Access outside of OpenSSL. I recognized three Root CAs during my inspection: GTS Root R4 (Issuer = GTS Root R4), DigiCert Trusted Root G4 (Issuer = DigiCert Trusted Root G4), and GlobalSign (Issuer = GlobalSign).

4.Finally, I used OpenSSL to verify that the google.com certificate was included in my trust store. I received an output stating “Verify return code: 0 (ok),” which confirmed that the certificate is trusted and located in my operating system’s trust store.

## Results
- There are 154 Root CAs in my operating system’s trust store, which shows that my system already trusts many certificate authorities by default. These Root CAs are responsible for verifying the identity of websites and ensuring secure connections.
  
- One Root CA I inspected directly in my trust store via Keychain Access is GTS Root R4. The issuer for this CA is also GTS Root R4, meaning it is self-signed. This confirms that it is a trusted root certificate, as root CAs sign their own certificates and are placed in the trust store by the operating system.
  
- The verify return code output showed that google.com is validated in my trust store. This means that the certificate chain for google.com is trusted because it links back to a Root CA that already exists in my system’s trust store.

 List Of Root Ca's
 
<img width="816" height="952" alt="root_cas" src="https://github.com/user-attachments/assets/ced3e43f-9870-47c7-a721-977a964ca172" />


## Key Findings

Through this lab, I learned that trust stores play a major role in how systems verify certificates and establish secure connections. My operating system already contains many pre-installed Root CAs, which allows it to trust certificates from websites without needing manual approval.

## Explanation
- My browser trusts the certificate because it is issued by a Certificate Authority that already exists in my operating system’s trust store. Since the root CA is already trusted, any certificate that chains back to it is automatically trusted.
  
- If an internal root CA is not in the trust store, my system would not trust any certificates issued by it. This would cause errors or warnings when trying to access internal systems because the certificate chain cannot be verified.
  
- What surprised me was how many Root CAs were already installed by default. I didn’t expect there to be that many trusted authorities on my system, which showed me how much trust is already built into the operating system.

## Challenges / Troubleshooting

Luckily, i faced no challenges during this lab

## Artifacts
- root_cas.pem (macOS) or equivalent, screenshots stored in assets/screenshots/week-04/

--

## Stretch

What Would Happen when an enterprise pushes an internal root CA to devices using Group Policy or an MDM?

An enterprise would push their own root CA to employee devices using Group Policy or MDM so that internal systems and applications can be trusted automatically. This allows employees to access company websites, VPNs, and services without getting security warnings, since those certificates will chain back to the organization’s root CA.

A security risk exists if an unauthorized root CA is added to a device because that root CA could be used to issue fake certificates. This means an attacker could intercept traffic, perform man-in-the-middle attacks, or make malicious websites appear trusted, which can compromise sensitive data.

To detect if an unexpected root CA has been installed on a machine, I could inspect the trust store using tools like Keychain Access on macOS or certificate management tools on Windows. I would look for unfamiliar or unrecognized certificate authorities and verify them. 
