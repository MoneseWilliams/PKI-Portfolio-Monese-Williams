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
- [Yes] Opened certsrv.msc via Run dialog
- [Yes] Confirmed CA name visible: CVI Issuing CA 1
- [Yes] Confirmed CA status: Running
- [Yes] Expanded left pane — folders visible (Revoked Certificates, Issued Certificates, etc.)

**Screenshot or description of what you observed in certsrv.msc:**

```
The Certification Authority console opened successfully. I confirmed that CVI Issuing CA 1 was visible. The left pane displayed folders including Issued Certificates, Revoked Certificates, Pending Requests, Failed Requests, and Certificate Templates.

```

---

## Step 5 – Certificate Log File Path

**Command run on PKI-SRV01:**

```powershell
Get-ChildItem "C:\Windows\System32\CertLog"
```

**Output received:**

```
 Directory: C:\Windows\System32\CertLog


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         5/16/2026   5:04 PM        1048576 CVI Issuing CA 1.edb
-a----         5/16/2026   5:04 PM          16384 CVI Issuing CA 1.jfm
-a----         5/16/2026   5:04 PM           8192 edb.chk
-a----         5/16/2026   5:04 PM        1048576 edb.log
-a----         4/25/2026   7:45 PM        1048576 edbres00001.jrs
-a----         4/25/2026   7:45 PM        1048576 edbres00002.jrs
-a----         4/25/2026   7:45 PM        1048576 edbtmp.log
-a----         5/16/2026   5:04 PM          20480 tmp.edb

```

**Files found in CertLog:**

| File Name | Approximate Size |
|-----------|-----------------|
|CVI Issuing CA 1.edb |1048576|
|CVI Issuing CA 1.jfm |16384|
|edb.chk |8192|
|edb.log |1048576|
|edbres00001.jrs |1048576|
|edbres00002.jrs |1048576|
|edbtmp.log |1048576| 
|tmp.edb |20480|

---

## Reflection

**One thing that went well during this lab:**

```
One thing that went well was me being able to login into all 3 systems with no issues, also being able to use powershell with no issues when needing to do the commands (Test-Connection -ComputerName DC01 -Count 2) and (Get-Service -Name CertSvc) with succussful outputs. 
```

**One thing that was confusing or unexpected:**

```
One thing that was confusing during this lab was attempting to run the command Get-ChildItem "C:\Windows\System32\CertLog" because it initially returned an “Access Denied” output. I realized I needed to run PowerShell as Administrator first in order for the command to work correctly. It became even more confusing because I am using a MacBook connected to an Azure virtual machine, and inside the virtual machine I am also using VirtualBox. Because of that setup, some Mac keyboard shortcuts were not working correctly in Windows. After troubleshooting, I figured out I could use fn + Command + R to search for PowerShell and then use Control + Shift + Return to launch PowerShell as Administrator. Once I did that, I was able to successfully run the command and retrieve the required output.

```

---

## Submission Checklist

- [Yes] All five steps completed
- [Yes] All command outputs pasted
- [Yes] Reflection section filled in
- [Yes] File saved as `lab-01-environment-verification.md`
- [Yes] File committed to my portfolio repo under `labs/week-09/`
