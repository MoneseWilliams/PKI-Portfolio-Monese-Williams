# Week X Reflection

Submit this in your portfolio repository:

`reflections/week-XX.md` (e.g. `reflections/week-03.md`)

---

## Prompts

1. his week I learned how to analyze a real enterprise certificate and actually see how PKI is used in a real-world environment. I retrieved a live certificate from Capital One and broke down everything from the certificate fields, the trust chain, and how it’s deployed. I also learned how to tell the difference between certificate types like EV and OV, and how to identify it using the certificate policy ID instead of just guessing based on the subject field. I also analyzed the full chain and confirmed it was complete from the leaf to the root CA. Another thing I learned was how to figure out where TLS is actually terminating. Based on my findings, it looks like TLS is terminating at a load balancer, especially because of the “BigIP” indicator, which is tied to F5 load balancers. I also used SSL Labs and certificate transparency logs, which helped me see how companies manage their certificates over time and how strong their security setup is.

2. The most challenging part for me was figuring out the difference between EV and OV certificates. At first, I thought just seeing organization info meant it was OV, but I learned that EV certificates are confirmed by the certificate policy ID, which cleared it up for me.
Another challenge was explaining everything I found in a way that actually makes sense. I understood what I was seeing, but putting it into clear technical wording took a little more effort.

3. This shows up in real-world systems with large companies like banks that need to secure their websites and protect user data. They use strong TLS configurations and trusted certificates to make sure everything is secure. It also shows up with load balancers and systems handling a lot of traffic, making sure connections stay secure and stable. Certificate transparency logs are also used to track certificates and make sure nothing suspicious is going on.

4. I would explain it like this: when you go on a website, there’s something behind the scenes making sure that website is actually who it says it is and that your information is safe.
Big companies have more advanced setups to handle this, but at the end of the day, it’s just about making sure you can trust the site and your data is protected.

5. Some questions I still have are how companies manage so many certificates at once and how they keep track of everything without missing anything. I also want to understand more about how they decide where TLS should terminate in different environments.

---

## Professional Growth Check

- [yes] I documented my work clearly and in my own words
- [yes] I used structured formatting in my submission files
- [yes] My commit message was meaningful and descriptive

---

*CVI PKI Career Pathway — Foundations Phase*
