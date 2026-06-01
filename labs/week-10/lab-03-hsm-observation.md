# Lab 03: HSM Key Ceremony — Observation Report

**Student Name:**  Monese Williams
**Date Completed:**  5/31/2026
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-03-hsm-observation.md`

> **Note:** This is an instructor-led demonstration. You observe and document — no hands-on configuration required. The instructor runs SoftHSM2 on a dedicated demo machine. Your deliverable is a structured observation report.

---

## Overview

A Hardware Security Module (HSM) is a physical device that stores cryptographic keys in tamper-resistant hardware. Enterprise CAs use HSMs to protect CA private keys — the most sensitive material in the PKI environment. In this demonstration, the instructor runs a **PKCS#11 key ceremony** using **SoftHSM2**, a software-based HSM simulator that exposes the same interface as a physical HSM like the Thales Luna.

Your task: observe each step, document what the instructor ran, what it produced, and what the equivalent step would look like with a real HSM in an enterprise environment.

---

## Part A — Setup Context

**What is SoftHSM2?**

In your own words, describe what SoftHSM2 is and why it is being used for this demonstration instead of a physical HSM:

```
A SoftHSM2 is more like files being stored on disk. The private key is stored on disk rather than in hardware and is exportable if configured to allow it. It is being used for this demonstration instead of a physical HSM because it's not really about the certificates themselves. It's more about what an attacker can do if they gain access to the server. SoftHSM2 is not a good choice for production environments because it can be copied off a server by anyone with administrative access, while HSM keys physically cannot. Once a private key is generated inside an HSM, you cannot even see the key itself.


```

**What is PKCS#11?**

In your own words, describe what the PKCS#11 interface is:

```
A PKCS#11 interface is used with HSMs to help generate and manage private keys. It is used for smart cards, HSMs, and security tokens. The CA would use the PKCS#11 interface to communicate with the HSM and perform cryptographic operations such as signing, decryption, and key generation instead of giving out the private key.


```

**HSM being simulated in this demo:**

| Item | Value |
|------|-------|
| Software used | SoftHSM2 (Windows) |
| PKCS#11 library path |softhsm2.dll|
| Companion tool used (key ceremony commands) |softhsm2-util|

---

## Part B — Step-by-Step Observation

Document each step of the ceremony as the instructor runs it. Record the command, what it did, and what you observed in the output.

---

### Step 1 — Token Slot Initialization

**Command run by instructor:**

```
softhsm2-util --init-token --slot 0 --label "CVI-
CAKeyStore" --so-pin <SO-PIN> --pin <user-
PIN>

```

**What this command does:**

```
This command initialized Slot 0 with the CVI CA Key Store and created two PINs. One PIN is used when needing to perform signing operations, and the other PIN is used by the Security Officer, who controls the administrative operations.

```

**Output or confirmation observed:**

```
The output of this command shows that two PINs were created. One PIN created is the user PIN, and this is what the CA software presents when needing to perform signing operations. The second PIN, which is the SO PIN, was created for the Security Officer and is a separate credential that controls the administrative operations on the token. The PINs are used as part of the quorum.


```

**Why this step is necessary:**

```

This is necessary because, in the real world, some people would have access to the administrative portion of operations, while others would have access to the signing portion of operations. This helps prevent one person from being able to do everything, which supports the best practice of least privilege and separation of duties.


```

---

### Step 2 — Confirm Slot/Token State

**Command run:**

```
softhsm2-util --show-slots 

```

**Output:**

```
The token has been initialized and is reassigned to slot 2076269524

[toniawebster@Mac ~ % softhsm2-util --show-slots

Available slots:
Slot 2076269524
    Slot info:
        Description:      SoftHSM slot ID 0x7bc15bd4
        Manufacturer ID:  SoftHSM project
        Hardware version: 2.7
        Firmware version: 2.7
        Token present:    yes

    Token info:
        Manufacturer ID:  SoftHSM project
        Model:            SoftHSM v2
        Hardware version: 2.7
        Firmware version: 2.7
        Serial number:    1ae762c17bc15bd4
        Initialized:      yes
        User PIN init.:   yes
        Label:            CVI-CAKeyStore

Slot 1
    Slot info:
        Description:      SoftHSM slot ID 0x1
        Manufacturer ID:  SoftHSM project
        Hardware version: 2.7
        Firmware version: 2.7
        Token present:    yes

    Token info:
        Manufacturer ID:  SoftHSM project
        Model:            SoftHSM v2
        Hardware version: 2.7
        Firmware version: 2.7
        Serial number:
        Initialized:      no
        User PIN init.:   no
        Label:

```

**What did the output confirm?**

```
The output confirms that Slot 0 now has a serial number and a slot number associated with it. It tells you that it is still on Slot 0 and still has the same hardware and firmware versions, but now it shows that there is a token present. This confirms that Slot 0 has been initialized with a label named CVI-CAKeyStore.


```

---

### Step 3 — Key Generation

**Command run:**

```
pkcs11-tool --module <library> --login --pin
<user-PIN> --keypairgen --key-type RSA:2048
--label "CVI-IssCA-Key" --id 01

```

**Key parameters used (record what you observed):**

| Parameter | Value |
|-----------|-------|
| Key type |Private Key RSA|
| Key size |2048|
| Key label |CVI-IssCA-Key|
| Token |0x7bc15bd4|

**Output:**

```
Using slot 0 with a present token (0x7bc15bd4)

Key pair generated:

Private Key Object: RSA 2048 bits
  Modulus:
    c25cafd44e2efe5d2152d4dae6fcd8fdd585780d24abd011e71246c3f7473abd
    34a7d49320f3e26ff796ee2926c9aa5330412047797ec137fc88e0d947359828
    ae2b375cf2348a12e873484256f961583e571c25b18acaaa107d30bcbb1c5974
    d6bc7ebb65abcced425328936b2934ea8f0cddbe19a2b0b6f0729339553df9cb
    f5c0d372aa868eb9ddc03b4ddb20b47be5a38854fb673ef429a2cbd923278531
    473e1b86318a07d5f6c438af66956495ef9da88b334b5886488519251850e804
    575fc0f70436bf657bba23f56bb64333cb69517430299fceac45cc1d72e685ae
    32047624a3ce4f2236bd08ff2c23af31f3ff7d45de294113f2687ddba161cef5

  Public exp: 65537 (0x010001)
  label:      CVI-IssCA-Key
  ID:         1 (0x01)
  Usage:      decrypt, sign, signRecover, unwrap
  Access:     sensitive, always sensitive, never extractable, local
  URI:        pkcs11:model=SoftHSM%20v2;manufacturer=SoftHSM%20project;serial=1ae762c17bc15bd4;token=CVI-CAKeyStore;id=%01;object=CVI-IssCA-Key;type=private

Public Key Object: RSA 2048 bits
  Modulus:
    c25cafd44e2efe5d2152d4dae6fcd8fdd585780d24abd011e71246c3f7473abd
    34a7d49320f3e26ff796ee2926c9aa5330412047797ec137fc88e0d947359828
    ae2b375cf2348a12e873484256f961583e571c25b18acaaa107d30bcbb1c5974
    d6bc7ebb65abcced425328936b2934ea8f0cddbe19a2b0b6f0729339553df9cb
    f5c0d372aa868eb9ddc03b4ddb20b47be5a38854fb673ef429a2cbd923278531
    473e1b86318a07d5f6c438af66956495ef9da88b334b5886488519251850e804
    575fc0f70436bf657bba23f56bb64333cb69517430299fceac45cc1d72e685ae
    32047624a3ce4f2236bd08ff2c23af31f3ff7d45de294113f2687ddba161cef5

  Public exp: 65537 (0x010001)
  label:      CVI-IssCA-Key
  ID:         1 (0x01)
  Usage:      encrypt, verify, verifyRecover, wrap
  Access:     local
  URI:        pkcs11:model=SoftHSM%20v2;manufacturer=SoftHSM%20project;serial=1ae762c17bc15bd4;token=CVI-CAKeyStore;id=%01;object=CVI-IssCA-Key;type=public

```

**What this step accomplished:**

```
It generated a new signing key pair, consisting of a private key and a public key, for the new Issuing CA. It can now be used to decrypt, sign, and recover data. The private key is not extractable, meaning it cannot be taken from the HSM or exported.


```

---

### Step 4 — Confirm Key is Token-Resident

**Command run:**

```
pkcs11-tool --module <library> --login --pin
<user-PIN> --list-objects

```

**Output:**

```
Using slot 0 with a present token (0x7bc15bd4)

Public Key Object: RSA 2048 bits
  Modulus:
    c25cafd44e2efe5d2152d4dae6fcd8fdd585780d24abd011e71246c3f7473abd
    34a7d49320f3e26ff796ee2926c9aa5330412047797ec137fc88e0d947359828
    ae2b375cf2348a12e873484256f961583e571c25b18acaaa107d30bcbb1c5974
    d6bc7ebb65abcced425328936b2934ea8f0cddbe19a2b0b6f0729339553df9cb
    f5c0d372aa868eb9ddc03b4ddb20b47be5a38854fb673ef429a2cbd923278531
    473e1b86318a07d5f6c438af66956495ef9da88b334b5886488519251850e804
    575fc0f70436bf657bba23f56bb64333cb69517430299fceac45cc1d72e685ae
    32047624a3ce4f2236bd08ff2c23af31f3ff7d45de294113f2687ddba161cef5

  Public exp: 65537 (0x010001)
  label:      CVI-IssCA-Key
  ID:         1 (0x01)
  Usage:      encrypt, verify, verifyRecover, wrap
  Access:     local
  uri:        pkcs11:model=SoftHSM%20v2;manufacturer=SoftHSM%20project;serial=1ae762c17bc15bd4;token=CVI-CAKeyStore;id=%01;object=CVI-IssCA-Key;type=public

Private Key Object: RSA 2048 bits
  Modulus:
    c25cafd44e2efe5d2152d4dae6fcd8fdd585780d24abd011e71246c3f7473abd
    34a7d49320f3e26ff796ee2926c9aa5330412047797ec137fc88e0d947359828
    ae2b375cf2348a12e873484256f961583e571c25b18acaaa107d30bcbb1c5974
    d6bc7ebb65abcced425328936b2934ea8f0cddbe19a2b0b6f0729339553df9cb
    f5c0d372aa868eb9ddc03b4ddb20b47be5a38854fb673ef429a2cbd923278531
    473e1b86318a07d5f6c438af66956495ef9da88b334b5886488519251850e804
    575fc0f70436bf657bba23f56bb64333cb69517430299fceac45cc1d72e685ae
    32047624a3ce4f2236bd08ff2c23af31f3ff7d45de294113f2687ddba161cef5

  Public exp: 65537 (0x010001)
  label:      CVI-IssCA-Key
  ID:         1 (0x01)
  Usage:      decrypt, sign, signRecover, unwrap
  Access:     sensitive, always sensitive, never extractable, local
  uri:        pkcs11:model=SoftHSM%20v2;manufacturer=SoftHSM%20project;serial=1ae762c17bc15bd4;token=CVI-CAKeyStore;id=%01;object=CVI-IssCA-Key;type=private

```

**What the output proves:**

```
The output demonstrated that the key lives in the HSM. It shows that the key is stored within the token and that the private key is marked as never extractable, meaning it cannot be exported from the HSM.

```

---

### Step 5 — Any Additional Steps Demonstrated

*(Use this section if the instructor ran additional commands — e.g., key attribute inspection, PKCS#11 URI display, CA configuration reference)*

**Command:**

```
softhsm2-util --show-slots

```

**What it showed:**

```
It showed the slots on the software, the manufacturer, and the hardware versions. It also showed the token that had not been initialized, meaning there is no private key associated with that token and it is a clean token. It also shows the token information and how there is no PIN associated with it. It's just a clean Slot 0.


```

---

## Part C — SoftHSM2 vs. a Physical HSM

**Physical HSM reference: Thales Luna Network HSM**

Fill in the table based on the lesson and demonstration:

| Dimension | SoftHSM2 (Demo) | Thales Luna (Enterprise) |
|-----------|-----------------|--------------------------|
| Key storage location | Software (filesystem) |Hardware Device HSM|
| Tamper resistance | None |Tamper Resistant|
| PKCS#11 interface | Same |Same|
| FIPS validation | Not applicable |Validated|
| Used in production CAs | No — simulation only |Yes|
| Cost | Free / open source |Expensive Hardware|
| Required for a CA private key | No | Common in enterprise PKI |

**In your own words: what does a physical HSM protect against that SoftHSM2 cannot?**

```
A physical HSM protects against potential attackers who somehow gain access to the server and private key. Unlike SoftHSM2, the private key cannot be extracted or copied from the HSM.

```

---

## Part D — The Key Ceremony Concept

**What is a key ceremony?**

In your own words, describe what a key ceremony is and why enterprise PKI deployments conduct them with multiple administrators present:

```
A key ceremony is the process of creating and protecting a CA's private key. Enterprise PKI deployments conduct key ceremonies with multiple administrators present so that one person does not have complete control over the process. This helps enforce separation of duties and reduces the risk of abuse or mistakes. The ceremony is usually documented to prove that proper security procedures were followed.

```

**What would be different about this ceremony if CVI Issuing CA 1 were using a physical HSM?**

```
If CVI Issuing CA 1 were using a physical HSM, multiple administrators would be present in the room during the key generation process. Physical security steps would be required, such as using administrator smart cards, tokens, or quorum authentication to access the HSM. The private key would be generated directly inside the HSM and would never leave the device. Documentation would be created to record who was present, what actions were performed, and that the ceremony was completed successfully.

```

---

## Part E — Connection to Phase 2

**Software-protected CA key vs. HSM-protected CA key**

CVI Issuing CA 1 in your lab environment stores its private key in software (the Windows CNG Key Storage Provider). Based on what you observed in this demonstration:

**What is the operational risk of a software-stored CA private key vs. an HSM-protected key?**

```
The operational risk of a software-stored CA private key is that an attacker who gains administrative access to the server may be able to copy or steal the private key. With an HSM-protected key, the private key cannot be extracted from the device, which provides much stronger protection.

```

**When you back up the CA in Week 13, what specific step is more sensitive because the private key is software-protected?**

```
When backing up the CA in Week 13, the most sensitive step is exporting the CA backup because certutil -backup can include the software-protected private key. If that backup falls into the wrong hands, the CA private key could potentially be compromised.

```

---

## Reflection

**The most important thing you took away from this demonstration:**

```
The most important thing I took away from this demonstration is the difference between a software-protected key and an HSM-protected key. I learned that with an HSM, the private key stays inside the device and cannot be exported, while a software-protected key can potentially be copied if an attacker gains access to the server.

```

**One question the demonstration raised that you want to understand better:**

```
One question this demonstration raised is how organizations back up and recover HSM-protected keys if the HSM fails or is destroyed, since the private key cannot be exported from the device.

```

---

## Submission Checklist

- [Yes] Part A: SoftHSM2 and PKCS#11 described in own words
- [Yes] Part B: All ceremony steps documented — commands, outputs, explanations
- [Yes] Part C: SoftHSM2 vs. Thales Luna comparison table completed
- [Yes] Part C: Physical HSM protection question answered
- [Yes] Part D: Key ceremony concept explained in own words
- [Yes] Part D: Enterprise ceremony differences described
- [Yes] Part E: Software key vs. HSM key risk addressed
- [Yes] Part E: Connection to Week 13 backup noted
- [Yes] Reflection completed
- [Yes] File saved as `lab-03-hsm-observation.md`
- [Yes] File committed to portfolio repo under `labs/week-10/`
