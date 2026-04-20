# Lab — Enterprise Certificate Analysis

## Overview
In this lab, I will be retrieving a live certificate from a well-known enterprise organization and performing a complete certificate analysis on the TLS certificate and its full deployment. I will be investigating where TLS terminates within the enviroment and documenting my findings.

---

## Steps Performed

1. First, I will be retrieving the live certificate from my target enterprise, Capital One Bank, using openssl s_client. I then viewed the live certificate to confirm it populated successfully and decided to pull up the full certificate chain for Capital One Bank. The full certificate TLS handshake completed succuessfully and populated three certificate blocks.

2. Next, i paresd the live certoficate 
3.
4.

---

## Results
Include the important outputs or findings from the lab.

Examples may include:

- Command outputs
- Certificate fields or values
- Verification results
- Screenshots (if applicable)

If you include screenshots, store them in `assets/screenshots/` at the root of your repo and reference them here.

**How to embed an image:**

**Option A — Terminal / Local Editor**

Save your screenshot to `assets/screenshots/` in your repo, then reference it using a relative path from your submission file:

```markdown
![Description of your screenshot](../../../assets/screenshots/your-filename.png)
```

> The `../../../` moves up three levels: `submissions/` → `week-03/` → `labs/` → repo root, then into `assets/screenshots/`.

**Option B — GitHub Web (Easiest)**

Open your `.md` file on GitHub, click the pencil icon to edit, then **drag and drop your image directly into the text editor**. GitHub will upload it automatically and insert the correct link for you.

Example of what an embedded image looks like:

```markdown
![Certificate output showing SAN field](../../../assets/screenshots/san-field.png)
```

---

## Key Findings
Document the most important observations from the lab.

Examples:

- What you discovered about the certificate, key, or protocol
- How a specific field or extension affected the outcome
- What a validation result indicated
- Any unexpected behavior or results

-
-
-

---

## Explanation
Explain **why the results matter**.

Examples:

- Why a specific field or extension is required
- Why a validation succeeded or failed
- What the result means in a real-world PKI context
- How this connects to the week's learning outcomes

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

Examples:

- Command errors
- Missing files or dependencies
- Verification failures and how you diagnosed them

---

## Artifacts
List the files generated or submitted during this lab.

Examples:

- Any `.pem`, `.crt`, or `.key` files produced
- Your completed lab write-up `.md` file
- Screenshots stored in `assets/screenshots/`

---

*CVI PKI Career Pathway — Foundations Phase*
