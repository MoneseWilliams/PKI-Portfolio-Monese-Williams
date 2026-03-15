# Lab — Symmetric Encryption (Confidentiality)

## Goal
This lab will help me udnerstand the process of how symmetric encryption is used to protect a file using AES and the importance of confidentiality within security.

---

## Part 1 — Setup

List any prerequisites or tools required.

- Use OpenSSl
- Access to a terminal using my Macbook
- created week 2 portfolio

All commands must be executed on my own computer 
GitHub cannot be used to run any programs, only storing files


---

## Part 2 — Execution Steps

1. First I cloned my Github Repository into my terminal by copying the link from the root of my directory and using the git clone command, after the clone was completed,  i opened my repository in my terminal and used the mkdir -p command to create the encrypted folder where the files from this lab will be stored
   
2. Next i created a plain text file using the echo command.  After running the command, I verified the filed existed and was readable 

3. Next, I used symmetric encryption though AES to encrypt the plaintext.txt file using the OpenSSL command. After running the command and creating my own password, a new file named plaintext.txt.enc was generated. I verified the file was their using the ls command, and verified it was unreadable and containing binary ciphertext using the cat command

4. I then made an attempt to decrypt the plaintext.txt file using the OpenSSL command again and inputting the password i created. After running that command, a new file was then generated again but named plaintext.decrypted.txt. i verified the file was theere using the ls command, and verified that my orignal text was successfully recovered using the cat command

5. Lastly, i used the diff command to compare the original plaintext file to the decrypted file. After running the command, no output appeared verifying the integrity of the two files. This verified that the encryption and decryption process worked correctly without any alterations to the original data

---

## Part 3 — Observations

Students should document:

- What happened
- Why it happened
- What surprised them
- Any errors encountered

---

## Submission (Portfolio Repo)

Create:

- labs/week-XX/[lab-name].md

Include:

- Screenshots (if applicable)
- Notes
- Short explanation of outcome

---

## Stretch (Optional)

Advanced challenge for stronger students.
