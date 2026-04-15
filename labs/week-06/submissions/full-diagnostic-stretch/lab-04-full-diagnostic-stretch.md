# Lab 04 — Full Diagnostic Scenario: Incident Report

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## PKI Incident Report

**System:** ehr.metrogeneral.org
**Reported:** Friday at 4:47 PM
**Author:** Monese Williams
**Status:** Pending remediation 

---

### Executive Summary

Metro General’s electronic health records are showing security errors, inhibiting staff from accessing patient records from the new clinical subnet. We need to perform a full diagnostic to determine the root cause of this error so it can be properly fixed.

---

### Technical Findings

#### Finding 1 — Certificate Is Within Validity Period

**Type:** Certificate
**Severity:** Low

**Detail:**
Inspection of the live certificate validity period field during my diagnostic indicates that the certificate is currently within its active validity window and has not expired. This rules out certificate expiration as the cause of the TLS failure.

**Evidence:**
The scenario states that the certificate was replaced last week and issued with a one-year validity window, indicating that the certificate should still be valid at the time of the incident.

---

#### Finding 2 — Server Presents Same Certificate Chain to Both Subnets

**Type:** Chain
**Severity:** [Critical / High / Medium / Low] Low

**Detail:**
Based on the diagnostic process and scenario details, I determined that the server presents the same certificate and certificate chain to both the working and failing subnets. This indicates the certificate chain itself is fully intact and is not the direct cause of the TLS failure.

**Evidence:**
This finding is supported by chain validation and the scenario statement that both subnets receive the same certificate and certificate chain from the server. Because the main office subnet successfully establishes a TLS connection using the same presented chain, this rules out a broken or incomplete certificate chain as the source of the issue.

---

#### Finding 3 — Missing Root CA in Clinical Subnet Trust Stores

**Type:** Trust Store
**Severity:** Critical

**Detail:**
After running the full PKI diagnostic framework and reviewing the given scenario, I determined that the internal Root CA was likely never pushed via Group Policy to the new clinical subnet that was added two weeks ago, because the original GPO push occurred six months earlier. This matters because the organization uses an internal CA, which must be trusted by client systems through manual distribution methods such as GPO. Without the internal Root CA present in the trust store, affected systems will fail TLS validation and receive certificate trust errors.

**Evidence:**
Scenario evidence supporting this finding includes the fact that the organization uses an internal CA rather than a public CA, meaning the internal Root CA must be distributed to company systems to establish trust. The scenario states the internal Root CA was pushed via GPO six months ago, while the failing clinical subnet was added only two weeks ago. This strongly suggests that devices connected to the clinical subnet may be missing the internal Root CA in their trust store.

---

### Diagnostic Steps

Document how you worked through the 4-step framework for this scenario.

#### Step 1 — Retrieve

First, the live certificate from the internal server ehr.metrogeneral.org needs to be retrieved using the command openssl s_client -connect ehr.metrogeneral.org:443 -showcerts. I would expect to receive output of the live certificate and the full trust chain within the TLS handshake so I can inspect the certificate. Since the server is accessible from one subnet but failing from another, my diagnostic approach would be to use the working subnet (10.10.0.0/24) as a baseline and compare it to the failing subnet (10.22.0.0/24) to identify what differences are causing the failure.

Based on the scenario, there is a reason to think the certificate was not successfully deployed because it was working fine before the replacement was implemented, and the replacement involved a new private key being generated as well.

---

#### Step 2 — Parse

Next, I would parse the certificate and inspect the Subject Key Identifier (SKI) to confirm whether a new key was used, since a new private key was generated during the replacement of the original certificate. I would also check the Issuer field to confirm the certificate was issued by Metro General Internal CA - G2, as stated in the scenario.

The information given in the scenario does not suggest a validity date problem, since the certificate was replaced last week with a one year validity period. However, I would still check the validity field to confirm that the dates are accurate. Overall, inspection of the certificate should rule out expired certificate issues.

---

#### Step 3 — Validate the Chain

Now, I would proceed with validating the certificate chain by using the command openssl verify -CAfile root.pem -untrusted intermediate.pem cert.pem. The chain validation should show that the chain is fully intact, ruling out broken chain issues since the certificate and the chain are the same in both subnets. This suggests the trust store is the source of the TLS failure on the clinical subnet because the GPO for the internal CA was pushed six months ago, but the clinical subnet was added two weeks ago. The state of the trust store on devices in the clinical subnet would mean that the internal root CA is missing from the trust store on those devices.


---

#### Step 4 — Check Revocation and Trust

Lastly, I would check revocation and trust of the certificate. Revocation checks would show whether the previously replaced certificate has been revoked by the issuing CA using CRL or OCSP status. Based on the scenario, since the certificate was fully replaced with a new certificate and new private key, there is an operational obligation to revoke the old certificate if it remains valid in order to prevent unauthorized use and reduce security risk.

The revocation status of the old certificate would not be relevant to the current incident because the outage is affecting the new certificate that has been deployed on the clinical subnet, not the old certificate. Overall, this step reinforces revocation as a secondary security concern rather than the cause of the current incident, because the revocation status of the old certificate would not impact the TLS failure occurring with the new certificate on the clinical subnet.

---

### Failures in Diagnostic Order

List each failure or contributing factor in the order a PKI engineer should address them:

1. **Missing Root Ca In Trust Store**
   - Type: Trust Store
   - Evidence: Based on the scenario, the internal Root CA was distributed to all devices via GPO six months ago; however, the clinical subnet was added to the network two weeks ago after the GPO push, suggesting this subnet missed the push to add the internal CA to its devices’ trust store.

2. **Missed GPO Internal CA Push**
   - Type: Trust Store
   - Evidence: The new clinical subnet was added two weeks ago, while the last GPO push occurred six months ago, strongly suggesting the organization did not perform an additional CA trust distribution to the newly added subnet, resulting in devices missing the internal Root CA.

---

### Root Cause

The underlying reason the incident occurred is because there was no imperative check to make sure that a GPO push for the internal CA was implemented for the new clinical subnet. This suggests the company has no checklist in place to validate and confirm that all subnets have received a GPO push for the internal CA after a new subnet is issued.

---

### Remediation Steps

List each action in the order it should be executed:

1. [Immediate — what restores access right now] An immediate GPO push of the internal Root CA to the clinical subnet so it can be added to the devices’ OS trust stores.
2. [Short-term — what fully resolves the incident within 24–48 hours] Validate that all subnets within the organization’s environment have received the GPO push for the internal Root CA and confirm the Root CA is actually present in systems’ trust stores.
3. [Secondary — cleanup, revocation, or follow-up actions] Ensure the previous certificate has been revoked to prevent unnecessary security risk if the old private key were ever compromised, and implement a system and/or process that requires automatic internal Root CA trust deployment whenever a new subnet is added.

---

### Prevention Recommendations

I recommend this organization implement a required validation checklist anytime a new subnet is added to make sure the internal Root CA is pushed and added to all systems’ trust stores before the subnet is fully added to the network for users. I also recommend implementing a system within GPO that automatically pushes the internal CA to newly added subnets so it does not have to be done manually.

---

### Lessons Learned

This incident revealed that the organization’s PKI operations lacked a structured process to ensure the deployment of the internal Root CA is implemented when a new subnet is added. Even though the internal CA was distributed six months ago with no issues via GPO, this new clinical subnet being added confirms that a strategy for ensuring all subnets receive the push or otherwise have the Root CA in their systems’ trust stores needs to be put in place.

---

## Reflection

The hardest part of the multi-failure investigation for me was not figuring out the issue itself, but explaining my reasoning in a technical way. Based on the scenario, I was able to work through the logic and determine what was happening, but putting that reasoning into proper technical wording for each framework step was the most difficult part for me. In a real production incident, I would continue improving my documentation skills so I can communicate technical findings as clearly as I can reason through them

---

*CVI PKI Career Pathway — Foundations Phase*
