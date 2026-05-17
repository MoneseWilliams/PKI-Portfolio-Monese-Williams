# Lab 01: Environment Verification & VM Connectivity Check

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 9  
**Submission Path:** `labs/week-09/lab-01-environment-verification.md`

---

## Step 1 – VM Startup & Login

**VMs started in correct order (DC01 → PKI-SRV01 → Root-CA):**
- Yes

**Login credentials used:**

| VM | Account Used | Login Successful? |
|----|-------------|-------------------|
| DC01 | CORP\pki.admin | Yes|
| PKI-SRV01 | CORP\pki.admin | Yes|
| Root-CA | .\Administrator | Yes|

**Notes / issues encountered:**

```
Root-CA System did take a while to start up after inputting the password, Overall i encontoured no issues when starting up all 3 systems.
```

---

## Step 2 – VM Connectivity Test

**Command run on PKI-SRV01:**

```powershell
Test-Connection -ComputerName DC01 -Count 2
```

**Output received:**

```
Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms)
------        -----------     -----------      -----------                              -----    --------
PKI-SRV01     DC01            192.168.10.10                                             32       1
PKI-SRV01     DC01            192.168.10.10                                             32       1

```

**DC01 responded successfully:**
- Yes
---

## Step 3 – CertSvc Service Status

**Command run on PKI-SRV01:**

```powershell
Get-Service -Name CertSvc
```

**Output received:**

```
Status   Name               DisplayName
------   ----               -----------
Running  CertSvc            Active Directory Certificate Services
```

**CertSvc status shown:** Running

**Service was Running:**
- Yes

---

## Step 4 – Certification Authority Console

**Steps completed on PKI-SRV01:**
- [ ] Opened certsrv.msc via Run dialog
- [ ] Confirmed CA name visible: ________________
- [ ] Confirmed CA status: ________________
- [ ] Expanded left pane — folders visible (Revoked Certificates, Issued Certificates, etc.)

**Screenshot or description of what you observed in certsrv.msc:**

```
<img width="773" height="542" alt="Screenshot 2026-05-16 at 7 08 52 PM" src="https://github.com/user-attachments/assets/18e8f361-5083-4752-92f6-d7ad9fd05c0a" />

```

---

## Step 5 – Certificate Log File Path

**Command run on PKI-SRV01:**

```powershell
Get-ChildItem "C:\Windows\System32\CertLog"
```

**Output received:**

```
(paste your output here)
```

**Files found in CertLog:**

| File Name | Approximate Size |
|-----------|-----------------|
| | |
| | |

---

## Reflection

**One thing that went well during this lab:**

```
(your response here)
```

**One thing that was confusing or unexpected:**

```
(your response here)
```

---

## Submission Checklist

- [ ] All five steps completed
- [ ] All command outputs pasted
- [ ] Reflection section filled in
- [ ] File saved as `lab-01-environment-verification.md`
- [ ] File committed to my portfolio repo under `labs/week-09/`
