# Lab — Hashing Integrity

## Overview
The purpose of thie lab is to understand how cryptographic hashing helps protect the integrity of transfered data. 

In this lab I generated a SHA-256 Hash file, I then modified the file by adding an additonal word to the message then generating a new hash file. I then compared both hash files and confirmed that they were not identical due to the modification

---

## Environment
Document the environment used to complete the lab.

- Operating System: macOS
- Terminal Used: Terminal
- OpenSSL Version (if applicable): OpenSSL 3.6.0

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

Do **not copy the lab instructions**.  
Describe what you actually did.

1.  First I accessed my repository on my terminal and created a file folder named hashes to store my lab artifacts. After that, I then proceeded to verify that the folder 'hashes' was succussfully created within the repository   
2.  Next, i wrote a text file "Week 2 Hashing Lab - CVI" into the hashes folder and then verified the file was created correctly and contained the message and readable
3.  Then, i generated a SHA-256 Hash file and viewed it. The Hash file showed a long string of unreadable text in hexadeciaml format which represents a unique digital footprint 
4.  Next, i modified the text file and included the word "tampered". I then verified the modification was implemented and proceeded to generate a new Hash file with the modified text and verified the new hash file and compared it to the first one which shows that one minor change did create a new digital fingerprint showing how hasing supports and verifies the integrity of data


---

## Results

The first SHA-256 hash was generated from the original message file "Week 2 Hashing Lab - CVI". After modifying the message to Week 2 Hashing Lab - CVI
tampered and generating a new hash file, the output of both values of both hash files are completely different. 

SHA2-256(labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.txt)= be2d7fb9708376211adb5b6034965a4b94c78ff42b14e7cbaf63fa02adbb21aa (Original)
SHA2-256(labs/02-week-02-cryptography-fundamentals/submissions/hashes/message.txt)= c628ac1a6cdb7ef27fd7e62c6e3208626ca24f76052d182a77e45e19c7546b93 (New)

the second hash file showed a completely different hexadecimal value from the original showing that even a small change to the text resulted into a new hash value.

Example:

![Hash Output](assets/screenshots/week-02/Hashing.png)

---

## Key Findings
Document the most important observations from the lab.

- The most important observations from this lab is that even a small modification to a file results in a completely different hash value.
- The SHA-256 hashing algorithm produces a fixed-length output regardless of the size of the input data.
- Hashing allows systems to detect whether data has been altered or tampered with.

---

## Explanation
Explain **why the results matter**.

The results of this lab matter because it showed the security property of integrity. Hashing helps ensures that data has not been altered during transmission. When a file is hashed, the value is basically a digital footprint of the data and if the file is modified in any way, the hash value will be changed which systems compare hash values fingerprints to determine whether the data has remained unchanged or not.

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

Once challenge due the lab was ensuring that the correct file path was used when generating the hash file. at first the commmand did not execute properly because the terminal was not in the correct directory. This was resolved by navigating to the correct folder in the terminal before running the OpenSSL command
Another challenge is creating the repo and where to submit my labs and everything included with it, i wouldnt say its resolved but hoping to have it resolved for my next week labs 

---

## Artifacts
List the files generated during this lab.

-message_tampered.sha256.txt
-message.sha256.txt
-message.txt
-assets/screenshots/week-02/Hashing.png

---

CVI PKI Career Pathway — Foundations Phase
