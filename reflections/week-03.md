# Week 3 Reflection

---

## Prompts

1. This week I’ve learned about X.509 certificate fields and their extensions. For example, Subject Alternative Name (SAN) is more sufficient than the CN field because it helps determine the identities of the certificate, and Basic Constraints helps determine whether or not the certificate can issue other certificates. I also learned how these fields are used during system verification through the trust chain to determine whether a connection can be trusted.
2. The most challenging concept for me was reading OpenSSL output and understanding how to build the certificate chain. At first, it was confusing trying to figure out which certificate was the root, intermediate, and leaf just by looking at the output. What made it difficult was that there were multiple certificate blocks and it wasn’t always clear how they were connected. I was able to understand it better by checking the subject and issuer fields and learning how the chain is built from the leaf to the intermediate and then to the root CA.
3. This shows up in real-world systems when secure connections are being made, like when accessing HTTPS websites. Systems use certificate chains to verify identity and make sure the connection can be trusted before allowing access.
4. I would explain it as a way of proving identity online. It’s like showing an ID to confirm who you are. The system checks different parts of the certificate to make sure it’s valid and trusted before allowing a secure connection.
5. One question I still have is how systems automatically retrieve missing intermediate certificates, and is it normal for most certificate to have missing root ca in openssl and have to make an attempt to find it in the trust root store and why ?

---

## Professional Growth Check

- Did you document clearly? yes
- Did you use structured formatting? yes
- Was your commit message meaningful? yes
