# Lab — Symmetric Encrypted (Confidentiality)

## Overview
the purpose of thie lab is to help me understand how Symmetric Encryption is used to protect data using AES and the importance of the security properity confidentiality.

---

## Environment
Document the environment used to complete the lab.

- Operating System: macOS
- Terminal Used:Terminal
- OpenSSL Version (if applicable): OpenSSL 3.6.0

---

## Steps Performed

1. First I cloned my Github Repository into my terminal by copying the link from the root of my directory and using the git clone command, after the clone was completed,  i opened my repository in my terminal and used the mkdir -p command to create the encrypted folder where the files from this lab will be stored
   
2. Next i created a plain text file using the echo command.  After running the command, I verified the filed existed and was readable 

3. Next, I used symmetric encryption though AES to encrypt the plaintext.txt file using the OpenSSL command. After running the command and creating my own password, a new file named plaintext.txt.enc was generated. I verified the file was their using the ls command, and verified it was unreadable and containing binary ciphertext using the cat command

4. I then made an attempt to decrypt the plaintext.txt file using the OpenSSL command again and inputting the password i created. After running that command, a new file was then generated again but named plaintext.decrypted.txt. i verified the file was theere using the ls command, and verified that my orignal text was successfully recovered using the cat command

5. Lastly, i used the diff command to compare the original plaintext file to the decrypted file. After running the command, no output appeared verifying the integrity of the two files. This verified that the encryption and decryption process worked correctly without any alterations to the original data

---

## Results

- After succuessfully doing this lab i was able to see that the file created was encryoted using AES-256 and became unreadable when opened. A password was then used to decrypt the file and convert it back to a readable file with the same data it obtained before. 

![Before Encryption](assets/screenshots/week-02/Before encrytion.png)
![Encryption](assets/screenshots/week-02/encryption.png)
![Decryption](assets/screenshots/week-02/decryption.png)


---

## Key Findings

- Symmetric Encryption uses one key to encrypt and decrypt the same file, protecting the data from attackers

- Both encrypted and decrypted files contained the same data with no modifications 

- When the wrong password is used the file will not be decrypted 

---

## Explanation

- The results matter because they demonstrate the security propertiy Confidentiality. For example, Symmetric Encryption is usdd to protect data from attackers while in transimittion and ensures that only users with the correct key will have access to the data and this will be important in the real world systems like TLS when a secure connection has been established for data transmittion to being.

---

## Challenges / Troubleshooting

-One challenge was understanding the correct commands and file paths when using OpenSSL. At times, the commands would not work due to incorrect paths. This was resolved by verifying my current directory and adjusting the file paths accordingly.


---

## Artifacts

- plaintext.txt
- plaintext.txt.enc
- plaintext.decrypted.txt

---

*CVI PKI Career Pathway — Foundations Phase*
