# Phase 1 Reflection Project

Submit this in your portfolio repository:
`reflections/phase1-reflection.md`

Include your visual in:
`reflections/visual/phase1-diagram.[png | pdf | jpg]`

---

## Before You Begin

Review all of your weekly reflections (`reflections/week-01.md` through `reflections/week-07.md`) before writing this.
Your final submission should build on those entries — not repeat them.

Choose your submission format:
- [ ] Written reflection (500–800 words)
---

## Part 1 — Your Phase 1 Journey

At the beginning of Phase 1, I didn’t really know anything about PKI. When I tried to learn about it on my own by doing quick research, I thought PKI was just making sure a certificate is valid by going through a checklist, and if it passed, you just move on. But as I went through each week, my understanding changed a lot. I started to see PKI as a full system that verifies identity and allows secure communication between systems.

One thing that really surprised me early on was how PKI is already built into our everyday systems, like browsers and operating systems. I also didn’t realize that root certificate authorities are already trusted by the OS, and that’s what allows us to trust websites automatically. Another thing that stood out to me was how PKI is basically invisible until something fails. That made me realize how important it actually is.

By the end of Phase 1, I stopped thinking of PKI as just certificates and started understanding it as a full system that connects identity, trust, and security together.

---

## Part 2 — How the Pieces Connect

All the concepts from Phase 1 connect together as one system. It starts with cryptography, which includes asymmetric encryption, symmetric encryption, and hashing. Asymmetric encryption is used to exchange keys securely using both the public and private key, while symmetric encryption is used for the rest of the TLS session because it’s faster and uses one key for both encryption and decryption. Hashing is used to make sure data isn’t changed during transmission.

Certificates are used to prove identity. They contain important information like the subject, issuer, public key, and X.509 extensions like SAN. These certificates are issued by certificate authorities and are verified through a trust chain that goes from the leaf certificate to the intermediate CA and then to the root CA.

The TLS handshake is where everything comes together. The server presents its certificate, the client verifies it using the trust chain, and if everything checks out, a secure session is created. If something is wrong, like a broken chain, expired certificate, or SAN mismatch, the connection fails.

The certificate lifecycle is also important, including key generation, certificate issuance, usage, renewal, and revocation. I also learned how troubleshooting fits into this system by diagnosing issues like expired certificates, missing intermediate certificates, and trust store problems using the PKI diagnostic framework.

Lastly, in real-world environments, PKI is used at scale with things like load balancers, internal certificate authorities, and monitoring systems. All of these pieces work together to make sure communication stays secure and trusted.

---

## Part 3 — A Lab That Made It Real

**Lab referenced:** `labs/week-06/submissions/broken-chain...` 

One lab that really made everything click for me was the broken chain lab. In this lab, I had to diagnose a TLS failure caused by a missing intermediate certificate. I retrieved the live certificate, parsed it, and then tried to validate the chain using OpenSSL.

At first, I saw two errors: “unable to get local issuer certificate” and “verification failed,” which helped me realize that the trust chain was incomplete. I then downloaded the correct intermediate certificate and used it to validate the chain again, which returned a successful result.

This lab showed me how important the trust chain is and how even one missing piece can cause the entire TLS connection to fail and be seen as unsecure. It also helped me understand how to troubleshoot issues step by step instead of guessing. It made PKI make more sense to me and gave me a glimpse of what a real PKI engineer might do on the job. Being able to diagnose and fix it myself felt like a real accomplishment.

---

## Part 4 — Explaining PKI to Someone Who Doesn't Know It

A lot of people think PKI is just SSL certificates, but that’s not the full picture. PKI is really about identity and trust, not just encryption, decryption, and certificates.

The easiest way I would explain it is that PKI is like a digital ID system for the internet. When you visit a website, your system checks that website’s certificate to make sure it is actually who it says it is. If everything matches and the source is verified, the connection is secure. If not, you will see a warning.

---

## Part 5 — Where You Go From Here

After learning about the PKI career path, I’m most interested in roles related to IAM and becoming a PKI Engineer. I like how PKI focuses on identity, trust, and security, and how it connects to many real-world systems.

The parts of Phase 1 that felt most relevant to me were troubleshooting, certificate validation, and understanding the TLS handshake. I enjoyed figuring out why things fail and how to fix them, especially in Week 6 where I worked through real scenarios.

Moving forward, I want to learn more about how PKI works in enterprise environments, especially how certificates are managed at scale and how automation is used. I also want to get better at explaining my technical findings clearly because I noticed that was and still is one of my biggest challenges.

Overall, Phase 1 gave me a strong foundation, and I feel more confident continuing into Phase 2.

---

## Required Visual

*Attach or embed your diagram below. Your visual must show PKI as a connected system. Choose one:*
- A TLS handshake with the full certificate chain labeled at each step
- A complete certificate lifecycle from key generation through revocation
- A PKI trust hierarchy with root CA, intermediate CA, and leaf certificate annotated

**Visual file:** `reflections/visual/phase1-diagram.[ext]`

This diagram shows the TLS 1.3 handshake between a client and a server. It starts with the client sending a Client Hello, followed by the server responding with sharing thier public key and certificate. during this process, the clients verifies the servers certifiacte using the certificate trust chain, which includes the intermediate and root ca. once verified succusfully, a secure session is establsihed, and the cleint and server can safleyly exchange HTTP requetes. This diagramshows how PKI, certificate and encryption all work together as on connected system.

---

## Second Lab Reference

*Reference at least one more lab — different from Part 3. What did you do and what did it reinforce?*

**Lab referenced:** `labs/week-05/submissions/expiration-stretch/...`

In this lab, I worked with an expired certificate and learned how expired certificates can cause systems to fail. I created certificates with short validity periods and even simulated an expired certificate to see what happens when a certificate is no longer valid.

When I verified the expired certificate, I saw errors showing that the certificate was no longer valid. I also used the OpenSSL command openssl x509 -checkend to check when a certificate will expire, which showed me how expiration can be predicted and monitored ahead of time by a specific amount of time.

This lab reinforced how important certificate lifecycle management is, especially in real-world environments where expired certificates can cause outages if they are not renewed on time.

---

## Professional Growth Check

- Did you write in your own voice — not a summary of the lessons? yes
- Did your reflection draw on your weekly entries rather than starting from scratch? yes
- Is your visual clearly labeled and accurate? yes
- Did you reference specific labs with file paths? yes
- Was your commit message meaningful? yes

---

*CVI PKI Career Pathway — Phase 1 Foundations*
