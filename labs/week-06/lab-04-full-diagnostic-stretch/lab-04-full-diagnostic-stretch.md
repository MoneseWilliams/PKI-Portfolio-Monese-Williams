# Lab 04 — Full Diagnostic Scenario: Multi-Failure Investigation (Stretch)

## Incident Report

**System:** Metro General EHR portal — ehr.metrogeneral.org

**Reported Behavior:** TLS failure for clinical staff on the 10.22.0.0/24 subnet; main office network unaffected

**Infrastructure Team's Note:** "The certificate was renewed last week. It was working fine before."

## Diagnostic Steps

Work through all four steps. For each, document what you checked and what it ruled in or out.

**Step 1 — Retrieve:**

**Step 2 — Parse:**

**Step 3 — Validate the Chain:**

**Step 4 — Check Revocation and Trust:**

## Findings — In Diagnostic Order

List all failures and contributing factors in the order a PKI engineer should address them.

**Finding 1 — Primary Failure**

- Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
- Severity:
- Detail:
- Evidence:

**Finding 2 — Contributing Factor**

- Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
- Severity:
- Detail:
- Evidence:

**Finding 3 — Additional Issue to Address**

- Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
- Severity:
- Detail:
- Evidence:

## Root Cause

Go beyond the technical failure. What process gap allowed this to happen? Why did clinical subnet devices end up in a different trust state than office devices?

## Remediation

Immediate steps (resolve the active TLS failure):

1.
2.

Follow-up steps (address contributing factors and prevent recurrence):

1.
2.
3.

## Key Findings

## Challenges / Troubleshooting

## Artifacts

- No certificate files required for this lab
