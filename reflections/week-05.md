# Week 5 Reflection

---

## Prompts

1. This week I learned about certificate revocation and expiration, and how both can impact systems in real-world PKI environments. I worked with OCSP and CRL to check whether a certificate has been revoked, and I learned that OCSP provides real-time status while CRLs are updated on a schedule.
I also learned how certificate expiration works by creating certificates with short validity periods and even expired certificates. I used OpenSSL commands like openssl x509 -checkend to check when a certificate will expire and understood how this can be used for monitoring and alerting in enterprise environments. Overall, I learned that even if a certificate was once valid, it can still fail due to expiration or revocation, which can cause systems to break if not properly managed.

2. The most challenging concept for me was understanding how to properly perform an OCSP query and why both the leaf certificate and the issuer certificate are required. At first, I didn’t fully understand why the issuer certificate was needed, but I learned that it is necessary to identify which certificate authority issued the certificate so the OCSP responder can check its status correctly. I also had some difficulty creating an expired certificate using OpenSSL because I initially used the wrong command options, which caused errors. After troubleshooting and reviewing the correct syntax, I was able to fix the issue and complete the lab.

3. This concept appears in real-world systems when websites and applications need to check whether a certificate is still valid or has been revoked. For example, when you visit an HTTPS website, the system may use OCSP to check the certificate status in real time. If the certificate is expired or revoked, the connection will fail or show a warning. Certificate expiration is also a major cause of outages in enterprise environments when certificates are not renewed on time, which can cause systems to stop working.

4. I would explain it by saying that certificates are like IDs for systems, but they don’t last forever and can also be taken away if something goes wrong. Systems have to regularly check if that ID is still valid and trusted. If the ID expires or gets revoked, the system will no longer trust it, and the connection will fail.

5. One question I still have is how enterprises manage and rotate certificates at scale without causing outages or failures. I also want to learn more about how trust stores are managed across different systems and how certificate issues are monitored in real time.

---

## Professional Growth Check

- [yes] I documented my work clearly and in my own words 
- [yes] I used structured formatting in my submission files
- [yes] My commit message was meaningful and descriptive

---

*CVI PKI Career Pathway — Foundations Phase*
