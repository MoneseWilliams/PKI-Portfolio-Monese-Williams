# Lab 02: AD CS Console Exploration & CA Hierarchy Documentation

**Student Name:**  Monese Williams
**Date Completed:**  5/15/2026
**Phase:** 2 | **Week:** 9  
**Submission Path:** `labs/week-09/lab-02-environment-documentation.md`

---

## Part A — AD CS Console Exploration (PKI-SRV01)

### CA Console Nodes — Observations

| Node | Contents / Observations |
|------|------------------------|
| Revoked Certificates | There are currently no revoked certificates displayed in this node. |
| Issued Certificates | There are currently no issued certificates displayed in this node. |
| Pending Requests | There are currently no pending certificate requests displayed in this node. |
| Certificate Templates | The currently published certificate templates on this CA include Directory Email Replication, Domain Controller Authentication, Kerberos Authentication, EFS Recovery Agent, Basic EFS, Domain Controller, Web Server, Computer, User, Subordinate Certification Authority, and Administrator|

### CA Properties — Key Settings

**General Tab**
- CA Name: CVI Issuing CA 1
- Computer Name: PKI-S4V01

**Extensions Tab — CRL Distribution Points (CDP):**

```
C:\Windows\System32\CertSrv\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl

ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>,CN=CDP,CN=Public Key Services,CN=Services,<ConfigurationContainer><CDPObjectClass>
```

**Extensions Tab — Authority Information Access (AIA):**

```
C:\Windows\System32\CertSrv\CertEnroll\<ServerDNSName>_<CaName><CertificateName>.crt

ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services,CN=Services,<ConfigurationContainer><CAObjectClass>
```

**Storage Tab**
- Database Path: C:\Windows\system32\CertLog
- Log Path: C:\Windows\system32\CertLog

### Certificate Templates Console (certtmpl.msc)

Templates visible in the forest (list what you observed):

```
Administrator
Authenticated Session
Basic EFS
CA Exchange
CEP Encryption
Code Signing
Computer
Cross Certification Authority
Directory Email Replication
Domain Controller
Domain Controller Authentication
EFS Recovery Agent
Enrollment Agent
Enrollment Agent (Computer)
Exchange Enrollment Agent (Offline request)
Exchange Signature Only
Exchange User
IPSec
IPSec (Offline request)
Kerberos Authentication
Key Recovery Agent
OCSP Response Signing
RAS and IAS Server
Root Certification Authority
Router (Offline request)
Smartcard Logon
Smartcard User
Subordinate Certification Authority
Trust List Signing
User
User Signature Only
Web Server
Workstation Authentication

```

---

## Part B — CA Hierarchy Verification (PKI-SRV01)

### Command: certutil -store -enterprise Root

```
Root "Trusted Root Certification Authorities"
================ Certificate 0 ================
Serial Number: 26373e51a6ab669340c47caef2232ce1
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
 NotBefore: 4/25/2026 6:15 PM
 NotAfter: 4/25/2046 6:25 PM
Subject: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Signature matches Public Key
Root Certificate: Subject matches Issuer
Cert Hash(sha1): b805e6ab548f6e7c57d3989f61de7fe6a51031d1
No key provider information
Cannot find the certificate and private key for decryption.
CertUtil: -store command completed successfully.
```

**What did you see?** (Subject, Issuer, Thumbprint — describe in your own words):

```
I observed that the Subject field showed `CN=CVI Root CA, DC=corp, DC=cvilab, DC=local` and the Issuer field contained the same information. This indicates that the certificate is self-signed, confirming that it is the trusted Enterprise Root Certificate Authority for the environment. I also observed the SHA1 certificate thumbprint, which is a hash value used for integrity purposes and help identifies the certificate.
```

### Command: certutil -store -enterprise CA

```
CA "Intermediate Certification Authorities"
================ Certificate 0 ================
Serial Number: 5800000002f7714edc7f317c46000000000002
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
 NotBefore: 4/25/2026 7:26 PM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Certificate Template Name (Certificate Type): SubCA
Non-root Certificate
Template: SubCA, Subordinate Certification Authority
Cert Hash(sha1): 5137a597de2c3085ec5816c7f11edc18cfcdbaf8
No key provider information
Missing stored keyset
CertUtil: -store command completed successfully.
```

**What did you see?** (Subject, Issuer, Thumbprint — describe in your own words):

```
For this Enterprise Intermediate Certification Authority, the Subject field showed `CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local`. The Issuer field showed that this certificate was issued by the Enterprise Root CA, `CN=CVI Root CA, DC=corp, DC=cvilab, DC=local`, confirming the trust relationship between the Root CA and the Intermediate CA. I also observed another SHA1 certificate thumbprint, which is a hash value used for integrity purposes for the certificate. Additionally, I observed that the certificate template being used was `Subordinate Certification Authority`.
```

---

## Part C — Active Directory Structure (DC01)

### Active Directory Users and Computers (dsa.msc)

**PKI Admins OU — accounts found:**

```
Cert Manager
PKI Admin
PKI Admins
```

**pki.admin account — group memberships:**

```
Domain Admins
Domain Users
PKI Admins
```

**cert.manager account — group memberships:**

```
Domain Users
PKI Admins

The `cert.manager` account is not a member of the `Domain Admins` group like the `pki.admin` account. This means the account has limited privileges and does not have full administrative permissions because it is not a member of the `Domain Admins` group.
```

**Domain-joined computer accounts found:**

```
DC01
PKI-SRV01
```

### Active Directory Sites and Services (dssite.msc)

**Server registered under Default-First-Site-Name:**

```
DC01
```

### Certificate Templates Console (certtmpl.msc on DC01)

Did templates appear here, confirming they are stored in AD?

- [No] — The Certificate Template console did not open successfully because Windows could not locate the console on the DC01 VM.

---

## Part D — Environment Summary Write-Up

> Write this section in your own words. Cover all five areas. Aim for clarity over length.

### 1. Environment Topology

The environment contains three virtual machines which are DC01, PKI-SRV01, and Root-CA. DC01 is the Domain Controller and handles Active Directory and authentication for the environment. PKI-SRV01 is the Enterprise Issuing CA that issues certificates throughout the environment. Root-CA is the offline Root Certificate Authority which is used to establish trust in the PKI environment. The systems are connected together through the lab network and communicate using private IP addresses.

### 2. CA Hierarchy

The Root CA named CVI Root CA was used to issue the Intermediate CA named CVI Issuing CA 1, which created the trust relationship between the two. The Root CA environment is kept offline because an attacker cannot compromise a CA that is not running. The Root CA should only be powered on for audited operations such as signing a new certificate or signing a new or renewed Issuing CA certificate. All trust within the PKI hierarchy comes from the Root CA, so if the Root CA is compromised, the entire PKI environment would need to be rebuilt.

### 3. Certificate Templates

The templates currently published to CVI Issuing CA 1 include templates such as Web Server, Computer, User, Domain Controller Authentication, Kerberos Authentication, and Subordinate Certification Authority. The Issuing CA also presented the SubCA and Subordinate Certification Authority template when running the PowerShell command "certutil -store -enterprise CA". These templates are designed to issue certificates for subordinate or intermediate Certification Authorities within the PKI hierarchy.

### 4. Active Directory Structure

The PKI Admins OU consists of Cert Manager, PKI Admin, and PKI Admins. The difference between the pki.admin and cert.manager accounts is that the pki.admin account is a member of Domain Admins, Domain Users, and PKI Admins which gives the account full administrative permissions within the environment. The cert.manager account is only a member of Domain Users and PKI Admins meaning the account has more limited permissions and does not have full domain administrative privileges.

### 5. One Thing I Found Interesting or Unexpected

One thing I found interesting during this lab was learning how the Root CA and Issuing CA trust relationship works together in the PKI hierarchy. One thing that was unexpected was troubleshooting the access denied error when trying to retrieve the CertLog files in PowerShell. Since I was using a MacBook with VirtualBox running inside an Azure virtual machine, figuring out the keyboard shortcuts and how to run PowerShell as Administrator became confusing at first, but I was eventually able to figure it out and complete the lab successfully.

---

## Submission Checklist

- [Yes] Part A: All five console nodes documented
- [Yes] Part A: CA Properties (CDP, AIA, Storage) recorded
- [Yes] Part A: Certificate Templates console observed
- [Yes] Part B: certutil -store -enterprise Root output included
- [Yes] Part B: certutil -store -enterprise CA output included
- [Yes] Part C: PKI Admins OU and both accounts documented
- [Yes] Part C: Sites and Services server noted
- [Yes] Part C: certtmpl.msc confirmed templates in AD
- [Yes] Part D: All five summary areas completed in own words
- [Yes] File committed to portfolio repo at `labs/week-09/lab-02-environment-documentation.md`
