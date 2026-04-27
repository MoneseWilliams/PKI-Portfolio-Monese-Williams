# PKI Troubleshooting Runbook

---

## Title

TLS Failure Due to SAN Hostname Mismatch

---

## Problem Statement

I expected the TLS connection to complete successfully when connecting to the server, but instead the connection failed due to a hostname mismatch error.

---

## Environment

**Tool(s) used:**

- OpenSSL
- Terminal

**Certificate type or component involved:**

- TLS Certificate
- Subject Alternative Name (SAN)

**Any relevant configuration details:**

- Server hostname was changed but the certificate SAN field was not updated to include the new hostname

---

## Symptoms

- Browser warning 
- TLS connection fails even though certificate appears valid
- Certificate shows valid issuer and dates but still not trusted

---

## Diagnostic Steps

*Walk through what you did to identify the root cause. Number each step. Include the exact commands you ran and what their output told you.*

1. 
2.
3.

---

## Resolution

*Describe exactly what fixed the issue — the specific command, config change, or action taken.*

> Write your response here.

---

## Prevention Note

*One to two sentences: how to avoid this issue in the future, or what to check first if it happens again.*

> Write your response here.

---

**Lab this scenario is drawn from:** `lab/XX-week-XX-[topic]/submissions/...`

---

*CVI PKI Career Pathway — Phase 1 Foundations*
