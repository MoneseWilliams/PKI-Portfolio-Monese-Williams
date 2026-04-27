# Week 6 Reflection

---

## Prompts

1. This week I learned how to diagnose and troubleshoot real PKI-related issues using a structured framework. I worked through different TLS failure scenarios such as expired certificates, broken certificate chains, and SAN hostname mismatches. I learned how to properly follow the PKI diagnostic steps: retrieve the certificate, parse it, validate the chain, and then check revocation and trust. This helped me understand how to break down a problem step by step instead of guessing. One of the biggest things I learned is that TLS failures can happen for different reasons, even if parts of the certificate look correct. For example, a certificate can be valid but still fail if the trust chain is broken or if the hostname does not match the SAN field.

2. The most challenging part for me was not identifying the issue, but explaining my reasoning in a clear and technical way. I was able to figure out what was wrong in each scenario, but putting that into structured documentation using the PKI diagnostic framework took more effort.
Another challenge was making sure I didn’t confuse different failure types, like thinking a chain issue was related to trust or revocation when it wasn’t. I had to slow down and really focus on each step to make sure I was identifying the correct root cause.

3. These concepts appear in real-world systems anytime secure connections fail. For example, users might see errors when accessing websites due to expired certificates, missing intermediate certificates, or hostname mismatches.
This also applies in enterprise environments where internal certificate authorities are used. If a root CA is not properly distributed to systems, it can cause trust failures and prevent users from accessing important services.

4. I would explain it by saying that secure connections depend on systems being able to trust each other. If something is wrong with that trust, like an expired ID, missing information, or a name that doesn’t match, the connection will fail.
It’s similar to trying to enter a building with an ID that is expired or doesn’t match your name. Even if everything else looks fine, access will still be denied.

5. Some questions I still have are how organizations handle troubleshooting at scale when multiple certificate issues happen at once, and how they automate detection for different types of failures like expiration, broken chains, or trust store issues.
I also want to get better at explaining my findings more clearly and professionally, especially in high-pressure situations like real incidents.

---

## Professional Growth Check

- [yes] I documented my work clearly and in my own words
- [yes] I used structured formatting in my submission files
- [yes] My commit message was meaningful and descriptive

---

*CVI PKI Career Pathway — Foundations Phase*
