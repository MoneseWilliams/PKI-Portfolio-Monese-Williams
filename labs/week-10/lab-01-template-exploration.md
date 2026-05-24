# Lab 01: Explore and Duplicate a Certificate Template

**Student Name:**  Monese Williams
**Date Completed:**  5/24/2026
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-01-template-exploration.md`

---

## Pre-Lab Verification

Run the following on PKI-SRV01 before starting. Do not proceed until all checks pass.

```powershell
# Check 1 — CA service running
Get-Service -Name CertSvc

# Check 2 — CA responding
certutil -ping

# Check 3 — Issuing CA cert in enterprise store
certutil -store -enterprise CA

```

**All checks passed:**
- [Yes] 

```
All checks passed. When I ran the first command, Get-Service -Name CertSvc, it showed that CertSvc was running, with the display name listed as Active Directory Certificate Services.
When I ran the next command, certutil -ping, it connected to the PKI-SRV01 server, “CVI Issuing CA 1,” and showed that the ping command was successful.
Lastly, when I ran certutil -store -enterprise CA, it populated the issuing CA certificate information, including the issuer, validity period, subject, and the certificate template used to create the certificate.

```

---

## Part A — Explore Three Built-in Templates

Open the Certificate Templates console: **Run → certtmpl.msc**

### Template 1: User

| Field | Value |
|-------|-------|
| Template Display Name |User|
| Template Name (internal) |User|
| Minimum Supported CA |Windows 2000|
| Validity Period |1 Year|
| Renewal Period |6 Weeks|

**General tab — notes:**

```
In the General tab, I can see that the template display name is “User,” which lets me know that the 33 templates under this group are User templates created in AD CS. The minimum supported CA for this template is Windows 2000.
I also observed that the validity period for this template is 1 year, while the renewal period is every 6 weeks. Lastly, I noticed there is a checked box labeled “Publish certificate in Active Directory,” which confirms that the built-in User certificate templates are published through Active Directory.

```

**Request Handling tab — Purpose:**

```
The Request Handling tab shows the purpose as “Signature and Encryption,” meaning the purpose of the built-in User certificate template is to allow users to digitally sign and encrypt data.

```

**Subject Name tab — Subject name format:**

```
In the Subject Name tab, the field “Build from this Active Directory information” should be checked, which it is. This means the certificate subject information, such as the user’s name or email, will automatically be populated from the user’s Active Directory account instead of being manually entered in the certificate request

```

**Extensions tab — Key Usage:**

```
Signature requirments: Digital Signature
Allow key exchange only with key encryption: critical extension

```

**Extensions tab — Application Policies (EKU):**

```
Encrypting File System
Secure Email
Client Authentication

```

---

### Template 2: Computer

| Field | Value |
|-------|-------|
| Template Display Name |Computer|
| Template Name (internal) |Machine|
| Validity Period |1 Year|
| Renewal Period |6 weeks|

**Request Handling tab — Purpose:**

```
In the Request Handling tab, it shows the purpose of the built-in Computer certificate template as “Signature and Encryption,” which is the same purpose shown in the User certificate template.

```

**Subject Name tab:**

```
n the Subject Name tab, the field “Build from this Active Directory information” should be checked, which it is. This means the certificate subject information will automatically be pulled from the computer’s Active Directory account instead of being entered manually.

```

**Extensions tab — Key Usage:**

```
Signature requirements:
Digital signature

Allow key exchange only with key encryption
Critical extension.

```

**Extensions tab — Application Policies (EKU):**

```
Client Authentication
Server Authentication

```

---

### Template 3: Web Server

| Field | Value |
|-------|-------|
| Template Display Name |Web Server|
| Template Name (internal) |Web Server|
| Validity Period |2 Years|
| Renewal Period |6 Weeks|

**Request Handling tab — Purpose:**

```
In the Request Handling tab, “Signature and Encryption” is shown as the purpose for the built-in Web Server certificate template, which is the same purpose listed in the User and Computer templates.

```

**Subject Name tab:**

```
For the Web Server template, “Supply in the request” will be populated in the Subject Name tab because web server certificate information is typically provided manually through the CSR (Certificate Signing Request) instead of automatically being pulled from Active Directory.

```

**Extensions tab — Key Usage:**

```
Signature requirements:
Digital signature

Allow key exchange only with key encryption
Critical extension.

```

**Extensions tab — Application Policies (EKU):**

```
Server Authentication

```

---

### Template Comparison

In your own words — what is the most significant difference between the User, Computer, and Web Server templates?

```
The most significant difference between the User, Computer, and Web Server templates is that the User and Computer templates both pull the subject information automatically from Active Directory, while the Web Server template requires the subject information to be entered manually through the CSR. This is because User and Computer are integrated with Active Directory, while web servers are not tied to Active Directory accounts.

```

Why does the Web Server template use "Supplied in the request" for the subject name rather than building it from Active Directory?

```
The Web Server template uses “Supply in the request” for the Subject Name instead of building it from Active Directory because web servers are not directly tied to Active Directory how user or computer account information are. The information needed for the certificate is typically provided through a CSR (Certificate Signing Request), which must be entered manually.

```

---

## Part B — Duplicate the Web Server Template

**Steps performed:**

1. Right-clicked the **Web Server** template → **Duplicate Template**
2. Selected compatibility settings:

   - Certification Authority: Windows Server 2012 R2
   - Certificate Recipient: Windows 7 / Server 2008 R2

3. Opened the properties of the new duplicate template.

**General tab — changes made:**

| Setting | Original Value | New Value |
|---------|---------------|-----------|
| Template display name | Web Server | CVI-WebServer |
| Template name | WebServer | CVI-WebServer |
| Validity period |2 Year|  | 1 year|
| Renewal period |6 Weeks|  | 6 weeks|

**Rationale for validity period chosen:**

```
I chose this validity period because a shorter certificate lifetime improves security by reducing the amount of time a potentially compromised certificate could be used. Having the validity period set to 1 year also helps ensure the certificate is renewed regularly to maintain security as well.

```

**Subject Name tab — changes made:**

| Setting | Change Made | Rationale |
|---------|-------------|-----------|
|Supply in| None        |This setting allows the subject information to be manually provided from CSR|
the request|

**Security tab — permissions confirmed:**

| Group / Account | Enroll | Autoenroll |
|-----------------|--------|------------|
| Domain Admins   |Yes     |No          |
| Authenticated Users |Yes |No           |

**Ensure Write and Enroll permissions are checked under Authenticated Users**

**Template saved:**
- [Yes] Yes — template visible in certtmpl.msc

---

## Part C — Inspect the Duplicate Template with certutil

Run the following command on PKI-SRV01:

```powershell
certutil -template CVI-WebServer
```

**Full output:**

```
 Name: Active Directory Enrollment Policy
  Id: {41635678-B3E8-4BD7-8FE7-D49A1E336991}
  Url: ldap:
34 Templates:

  Template[8]:
  TemplatePropCommonName = CVI-WebServer
  TemplatePropFriendlyName = CVI-WebServer
  TemplatePropSecurityDescriptor = O:S-1-5-21-3975454498-3980183307-2685672490-1105G:S-1-5-21-3975454498-3980183307-2685672490-519D:PAI(OA;;CR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;DC)(OA;;RPWPCR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;DA)(OA;;RPWPCR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;S-1-5-21-3975454498-3980183307-2685672490-519)(OA;;CR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;AU)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;DA)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;S-1-5-21-3975454498-3980183307-2685672490-519)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;S-1-5-21-3975454498-3980183307-2685672490-1105)(A;;LCRPWPLORCWDWO;;;AU)

    Allow Enroll        CORP\Domain Computers
    Allow Enroll        CORP\Domain Admins
    Allow Enroll        CORP\Enterprise Admins
    Allow Enroll        NT AUTHORITY\Authenticated Users
    Allow Full Control  CORP\Domain Admins
    Allow Full Control  CORP\Enterprise Admins
    Allow Full Control  CORP\pki.admin
    Allow Write NT AUTHORITY\Authenticated Users


CertUtil: -Template command completed successfully.

```

**From the certutil output — record the following:**

| Field | Value from certutil Output |
|-------|---------------------------|
| Template Name |CVI-WebServer|
| Template OID |Not Shown in the Output|
| Schema Version |Not Shown in the Output|
| Key Usage |Not Shown in the Output|
| Enhanced Key Usage (EKU) |Not Shown in the Output|
| Validity Period |Not Shown in the Output|
| Subject Name flags |Not Shown in the Output|

---

## Reflection

**Why does AD CS require you to duplicate a built-in template rather than modifying it directly?**

```
AD CS requires you to duplicate a built-in template instead of modifying it directly because the built-in templates are default templates that may already be used throughout the environment for other certificates. Modifying them directly could potentially affect existing systems, and creating a duplicated template will not affect the built-in templates or the rest of the environment.

```

**One setting in the template you found unexpected or would want to explore further:**

```
One setting in the template I would want to explore further is the Cryptography tab because it controls the cryptographic providers, minimum key size, and encryption settings used for the certificate. I would want to learn more about why certain providers, such as Microsoft RSA SChannel Cryptographic Provider and Microsoft DH SChannel Cryptographic Provider, are selected and what they even are.

```

---

## Submission Checklist

- [Yes] Pre-lab verification completed and outputs recorded
- [yes] Part A: All three templates explored with tab-level observations
- [Yes] Part A: Comparison question answered in own words
- [Yes] Part B: Duplicate template created as CVI-WebServer
- [Yes] Part B: All changes documented with rationale
- [Yes] Part C: certutil -template output pasted and key fields extracted
- [Yes] Reflection section completed
- [Yes] File saved as `lab-01-template-exploration.md`
- [Yes] File committed to portfolio repo under `labs/week-10/`
