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
| PKCS#11 library path | |
| Companion tool used (key ceremony commands) | |

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
create a key container within the HSM which is called the security officer

```

**Output or confirmation observed:**

```
(paste or describe output)
```

**Why this step is necessary:**

```
This step is necessary because the Security Officer is the person responsible for creating and initializing the token. In production environments, the Security Officer role would typically be part of the quorum. This is important because it allows access to the private keys. The token acts like a key store, while the container is where the keys actually live.


```

---

### Step 2 — Confirm Slot/Token State

**Command run:**

```
softhsm2-util --show-slots 

```

**Output:**

```
(paste output)
```

**What did the output confirm?**

```
(your observation)
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
| Key type | |
| Key size | |
| Key label | |
| Token | |

**Output:**

```
(paste output)
```

**What this step accomplished:**

```
(your explanation)
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
(paste output)
```

**What the output proves:**

```
(describe what it demonstrates about where the key lives)
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
| Key storage location | Software (filesystem) | |
| Tamper resistance | None | |
| PKCS#11 interface | Same | |
| FIPS validation | Not applicable | |
| Used in production CAs | No — simulation only | |
| Cost | Free / open source | |
| Required for a CA private key | No | Common in enterprise PKI |

**In your own words: what does a physical HSM protect against that SoftHSM2 cannot?**

```
(your answer here — 2–3 sentences)
```

---

## Part D — The Key Ceremony Concept

**What is a key ceremony?**

In your own words, describe what a key ceremony is and why enterprise PKI deployments conduct them with multiple administrators present:

```
(your answer — 3–5 sentences)
```

**What would be different about this ceremony if CVI Issuing CA 1 were using a physical HSM?**

```
(your answer — think about: who would be in the room, what physical steps would be required, what documentation would be produced)
```

---

## Part E — Connection to Phase 2

**Software-protected CA key vs. HSM-protected CA key**

CVI Issuing CA 1 in your lab environment stores its private key in software (the Windows CNG Key Storage Provider). Based on what you observed in this demonstration:

**What is the operational risk of a software-stored CA private key vs. an HSM-protected key?**

```
(your answer)
```

**When you back up the CA in Week 13, what specific step is more sensitive because the private key is software-protected?**

```
(your answer — think about what certutil -backup exports)
```

---

## Reflection

**The most important thing you took away from this demonstration:**

```
(your answer)
```

**One question the demonstration raised that you want to understand better:**

```
(your question)
```

---

## Submission Checklist

- [ ] Part A: SoftHSM2 and PKCS#11 described in own words
- [ ] Part B: All ceremony steps documented — commands, outputs, explanations
- [ ] Part C: SoftHSM2 vs. Thales Luna comparison table completed
- [ ] Part C: Physical HSM protection question answered
- [ ] Part D: Key ceremony concept explained in own words
- [ ] Part D: Enterprise ceremony differences described
- [ ] Part E: Software key vs. HSM key risk addressed
- [ ] Part E: Connection to Week 13 backup noted
- [ ] Reflection completed
- [ ] File saved as `lab-03-hsm-observation.md`
- [ ] File committed to portfolio repo under `labs/week-10/`
