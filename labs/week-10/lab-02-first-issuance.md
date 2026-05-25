# Lab 02: Issue Your First Certificate from a Custom Template

**Student Name:**  Monese Williams
**Date Completed:**  5/24/2026
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-02-first-issuance.md`

---

## Pre-Lab Verification

Run on PKI-SRV01 before starting.

```powershell
Get-Service -Name CertSvc
certutil -ping
```

**CertSvc status:** Running 
**CA responding (certutil -ping):**
- [Yes] 

**CVI-WebServer template visible in certtmpl.msc (from Lab 01):**
- [Yes]

---

## Part A — Publish the Template to the CA

The CVI-WebServer template exists in Active Directory but is not yet published to CVI Issuing CA 1. Publishing it makes it available for certificate requests.

**Steps performed on PKI-SRV01:**

1. Opened **certsrv.msc**
2. Expanded **CVI Issuing CA 1** → right-clicked **Certificate Templates** → **New → Certificate Template to Issue**
3. Selected **CVI-WebServer** from the list
4. Clicked **OK**

**CVI-WebServer template now visible under Certificate Templates node:**
- [Yes] Yes
- 
```
The duplicate template I created named CVI-WebServer successfully populated under the Certificate Templates node after issuing the template.

```

**Screenshot or description of the Certificate Templates node showing CVI-WebServer:**


<img width="764" height="539" alt="cert nodes" src="https://github.com/user-attachments/assets/9f328af1-bbf2-48b5-b73d-759864feea77" />


---

## Part B — Request the Certificate via MMC

**Steps performed on PKI-SRV01, logged in as CORP\pki.admin:**

1. Opened **mmc.exe** → **File → Add/Remove Snap-in**
2. Added **Certificates** snap-in
3. Selected: My User Account (Computer account / My user account / Service account)
4. Navigated to **Personal → Certificates**
5. Right-clicked → **All Tasks → Request New Certificate**
6. Proceeded through the Certificate Enrollment wizard

**Certificate Enrollment wizard — enrollment policy selected:**

```
In the certificate enrollment wizzard the policy i selected is Active Directory Enrollment Policy.
```

**Templates shown in the wizard:**

```
User
Computers

```

**CVI-WebServer template visible:**
- [ No] - troubleshooting steps taken: Based on the instructions given, I was advised to select “My user account” during the Add/Remove Snap-in process. However, once I went to request a new certificate and locate the duplicate template I created named CVI-WebServer, it populated as “Unavailable” with the error message: “This type of certificate can be issued only to a computer.”
At first, I deleted my duplicate template, recreated it, republished it, and followed the steps again, but the same issue continued to happen. I then researched the issue and found that it could be related to enrollment policy caching. To troubleshoot further, I opened Command Prompt (CMD) as administrator and ran the commands:

gpupdate /force
and
certutil -pulse

These commands were used to force a Group Policy refresh and refresh the certificate autoenrollment policy. After running both commands, I attempted to request the new certificate again, but unfortunately the issue still did not resolve.

After contacting a fellow classmate and the teacher’s assistant, I was advised not to select “My user account” during the Add/Remove Snap-in process as the instructions directed. Instead, I was told to choose “Computer account.” This made more sense because the error specifically stated that “This type of certificate can be issued only to a computer.”

Once I selected “Computer account,” I chose “Local computer,” then navigated back to Certificates → Personal → Request New Certificate. I again selected Active Directory Enrollment Policy in the enrollment wizard, and the CVI-WebServer template populated successfully. There was still an error shown under the template, so I clicked the warning, went to the Subject tab, entered the Common Name as:

CN=CVI-WebServer
then clicked Apply and Enroll.

The certificate enrollment then showed “Succeeded,” and my CVI-WebServer certificate now appears in the Personal certificate folder showing it was issued by CVI Issuing CA 1.

**Subject name entered (if prompted):**

```
The subjet name i provided is "CVI-WebServer"
```

**Certificate request submitted:**
- [Yes] Yes — certificate issued immediately

```
(paste error here if applicable)
```

---

## Part C — Inspect the Issued Certificate

### In the MMC Certificates Snap-in

Navigate to the Personal → Certificates store and double-click the issued certificate.

**General tab:**

| Field | Value |
|-------|-------|
| Issued to |CVI-WebServer|
| Issued by |CVI Issuing CA 1|
| Valid from |5/25/2026|
| Valid to |4/25/2027|

**Details tab — record the following fields:**

| Field | Value |
|-------|-------|
| Serial Number |440000000344d2f0d2f2690164000000000003|
| Signature Algorithm |sha256RSA|
| Subject |CN = CVI-WebServer|
| Key Usage |Digital Signature, Key Encipherment (a0)|
| Enhanced Key Usage |Server Authentication (1.3.6.1.5.5.7.3.1)|
| Subject Alternative Name (if present) | |
| Thumbprint |cfbcb10fadcc4ca02dbd4a8f2abb690238527bee|

---

### Via certutil

Export the certificate thumbprint from the Details tab, then run:

```powershell
certutil -store My "<thumbprint>"
```

Replace `<thumbprint>` with the thumbprint value (no spaces).

**Full certutil output:**

```
My "Personal"
================ Certificate 0 ================
Serial Number: 440000000344d2f0d2f2690164000000000003
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
 NotBefore: 5/25/2026 10:44 AM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=CVI-WebServer
Non-root Certificate
Template: CVI-Webserver, CVI-WebServer
Cert Hash(sha1): cfbcb10fadcc4ca02dbd4a8f2abb690238527bee
  Key Container = 328fd61f6686e813648820cf5fc82916_f0a99c17-76d3-498a-97de-2992c06105fd
  Provider = Microsoft RSA SChannel Cryptographic Provider
Missing stored keyset
CertUtil: -store command completed successfully.

```

---

### In certsrv.msc — Issued Certificates Node

Navigate to **certsrv.msc → CVI Issuing CA 1 → Issued Certificates**.

**Does the certificate appear in the Issued Certificates node?**
- [Yes] 

**Record from the Issued Certificates node:**

| Column | Value |
|--------|-------|
| Request ID |3|
| Requester Name |CORP\PKI-SRV01$|
| Certificate Template |CVI-WebServer (1.3.6.1.4.1.311.21.8.158)|
| Issued Common Name |CVI-WebServer|
| Certificate Expiration Date |4/25/2026 7:36 PM|

---

## Part D — Write-Up: The Issuance Workflow

Describe the full certificate issuance workflow in your own words. Cover:

1. What happened in Active Directory when you published the template
2. What the MMC Certificate Enrollment wizard sent to the CA
3. What the CA evaluated before issuing the certificate
4. Where the issued certificate was placed and why

```
During this full certificate issuance, I first published my duplicate Web Server template named CVI-WebServer to Active Directory because even though the actual template already existed in AD, publishing it made it available for certificate requests through the CA. Next, I went to the MMC (Microsoft Management Console) and performed a certificate snap-in for my computer, which is best for manual requests. A CSR request then needed to be submitted through the enrollment wizard so the MMC wizard could submit a PKCS#10 Certificate Signing Request with the subject name and public key information and send this request to the issuing CA.

Once the CSR was submitted to the issuing CA, the CA then performed a policy check, verifying the identity of the requester. Each check acted as a gateway to the next, and if all checks passed, the issuing CA then used its private key to sign the certificate and successfully issue it. Once the certificate was issued, it was placed in the local certificate store (Personal) because the certificate was issued locally through the computer account. After issuing, you always want to verify the information is correct, so within the Issued Certificates node in certsrv.msc, the information was verified and matched correctly. Also, in PowerShell, the command certutil -store My "thumbprint" displays all fields in the output, verifying the information matches the requested certificate as well.

```

**One thing about the issuance process that you did not expect or want to understand better:**

```
The issuance process that I did not expect and would like to understand better is what actually happens between the request and the certificate issuance. I also wonder if it is the same as the TLS handshake or if it is different since the certificate was being issued locally. Another thing I would like to understand better is why the duplicate template I created could only be used locally through the computer account instead of under “My user account” in MMC.
```

---

## Submission Checklist

- [yes] Pre-lab verification completed
- [yes] Part A: CVI-WebServer template published to CVI Issuing CA 1
- [yes] Part A: Template visible in certsrv.msc Certificate Templates node — confirmed
- [yes] Part B: Certificate requested via MMC — request submitted
- [yes] Part B: Enrollment wizard observations documented
- [yes] Part C: Certificate details recorded from MMC (General + Details tabs)
- [yes] Part C: certutil -store My output pasted
- [yes] Part C: Certificate confirmed in certsrv.msc Issued Certificates node
- [yes] Part D: Issuance workflow write-up completed in own words
- [yes] File saved as `lab-02-first-issuance.md`
- [yes] File committed to portfolio repo under `labs/week-10/`
