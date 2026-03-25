# Week 3 Lesson Notes — X.509 Certificate Anatomy

## 1. Core Concept

This week was about understanding how X.509 certificate fields like SAN, EKU, issuer, and validity period are used to verify identity within a system and establish a secure TLS connection.
---

## 2. Why It Matters

This matters because in real environments, certificates are used to secure websites and verify identities. If something is misconfigured, like a missing intermediate CA or an expired certificate, it can cause company outages or security warnings when accessing an HTTPS website.

---

## 3. Technical Breakdown

A certifiacte is a digital file used to verify identities. It includes fields like subject alternative name (SAN), extended key usage (EKU), and basic constraints which helps define a certificates main purpose, role, and what domains are attached to it. During a TLS connection, the server advises of who they are and the client issues the certificate. Trust is established when the system is able to validate the trust chain within the certifacte, but if one check fails during the verification process then the entire TLS conection will fail.

## 4. Common Misconceptions

One misconception I had was assuming that when using OpenSSL to view a certificate chain, the third or last certificate block would always be the root CA. As I worked through the labs, I learned that to correctly identify a root CA, you have to check whether the certificate is self-signed by comparing the subject and issuer fields. If they do not match, then the certificate is not the root CA. In that case, you may need to check your system’s trust store to find the actual root. If they do match, then you have correctly identified the root CA instead of just assuming.

Another misconception I had was thinking that if a certificate exists, it will automatically be trusted. But I learned that every part of the certificate has to be correctly configured, including the certificate chain, SAN, EKU, and validity period. If anything is missing or misconfigured, the certificate will fail validation and the connection will not be trusted.

---

## 5. Where This Shows Up

This tends to show up in web security through HTTPS connections. It is also used in internal enterprise systems to help secure applications and VPN connections, ensuring only trusted users and systems can connect. 

---

## Mental Model

PPKI is all about verifying identity, establishing trust, and making sure everything is properly validated before a secure connection is allowed. A certificate proves who a system says it is, the trust chain connects that identity back to a trusted root, and the verification process checks that everything is configured correctly. If all of those pieces align, the connection is trusted, but if anything is missing or misconfigured, trust is broken and the connection fails.
