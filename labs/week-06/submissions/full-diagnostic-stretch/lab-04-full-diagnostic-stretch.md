# Lab 04 — Full Diagnostic Scenario: Incident Report

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## PKI Incident Report

**System:** ehr.metrogeneral.org
**Reported:** Friday at 4:47 PM
**Author:** Monese Williams
**Status:** [Diagnosis complete — pending remediation / Resolved]

---

### Executive Summary

Metro General’s electronic health records are showing security errors, inhibiting staff from accessing patient records from the new clinical subnet. We need to perform a full diagnostic to determine the root cause of this error so it can be properly fixed.

---

### Technical Findings

#### Finding 1 — [Descriptive title, e.g., "Missing Root CA in Clinical Subnet Trust Stores"]

**Type:** [Certificate / Chain / Trust Store / Revocation / Configuration]
**Severity:** [Critical / High / Medium / Low]

**Detail:**
[What you found and why it matters technically]

**Evidence:**
[Commands run or scenario information that confirms this finding]

---

#### Finding 2 — [Descriptive title]

**Type:** [Certificate / Chain / Trust Store / Revocation / Configuration]
**Severity:** [Critical / High / Medium / Low]

**Detail:**
[What you found and why it matters technically]

**Evidence:**
[Commands run or scenario information that confirms this finding]

---

#### Finding 3 — [Descriptive title, if applicable]

**Type:** [Certificate / Chain / Trust Store / Revocation / Configuration]
**Severity:** [Critical / High / Medium / Low]

**Detail:**
[What you found and why it matters technically]

**Evidence:**
[Commands run or scenario information that confirms this finding]

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

1. **[Primary failure]**
   - Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
   - Evidence: [what in the scenario supports this]

2. **[Contributing factor or secondary issue]**
   - Type:
   - Evidence:

3. **[Additional issue, if applicable]**
   - Type:
   - Evidence:

---

### Root Cause

[One paragraph: what is the underlying reason the incident occurred? Go beyond the technical
failure — explain the process or operational gap that allowed it to happen.]

---

### Remediation Steps

List each action in the order it should be executed:

1. [Immediate — what restores access right now]
2. [Short-term — what fully resolves the incident within 24–48 hours]
3. [Secondary — cleanup, revocation, or follow-up actions]

---

### Prevention Recommendations

[2–3 concrete recommendations to prevent recurrence. Think about: certificate deployment
verification, Group Policy or MDM scope, post-renewal validation checklists, monitoring.]

---

### Lessons Learned

[One paragraph written as if debriefing your team. What did this incident reveal about the
organization's PKI operations? What should change going forward?]

---

## Reflection

[2–3 sentences: Which part of the multi-failure investigation was hardest to reason through?
Was there a point where you wanted to skip a framework step? What would you do differently
in a real production incident?]

---

*CVI PKI Career Pathway — Foundations Phase*
