# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab focused on certificate misconfigurations and how they cause validation failures. I explored how issues like missing intermediates, incorrect extensions, and expired certificates break trust in PKI.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**
Subject: CN=example.com

Modern browsers will not trute this certificate without the Subject Alternative Name (SAN)

**Analysis:**
SAN is a very important extension used in browsers. SAN lists the identites the certificate represents and allows multiple identities to exist in one certificate making the CN insufficatent alone. When a browser connectes to a website to its going to the check the SAN to see if the domain begin requeted appears on the list, if missing the certificate would be rejected

--

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**
Extended Key Usage: Client Authentication

A browser would not accept a certificate with the incorrect Extended Key Usage (EKU) for HTTPS

**Analysis:**
EKU defines what a certificate can be used for. For HTTPS, it must include Server Authentication which is required for HTTPS because it proves the server is trusted and legitimate. If it does not, the browser will reject the certificate and show a security error.

--

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**
Not Before: May 1 2022
Not After:  May 1 2023

The certificate would fail validation because it is expired. Since the “Not After” date is May 1, 2023, the certificate is no longer considered valid for use today.

**Analysis:**
The certificate would fail validation because it is expired. Certificates are only valid within their set time period, so once expired, they are no longer trusted. This is why lifecycle management is important to ensure certificates are renewed on time. Users would see a browser security warning and may be blocked from accessing the site.

---

## Scenario 4 — Missing Intermediate Certificate

**Can the browser build a complete trust chain?**
No, the browser cannot build a complete trust chain if the intermediate certificate is not provided by the server.

**Analysis:**
The server must provide it to link the leaf certificate to a trusted root CA. This is fixed by including the full certificate chain on the server.

---

## Key Takeaway
The most important thing I learned about certificate misconfigurations is that nothing required in the certificate can be missing. Every part of the certificate, including the intermediate certificate, SAN, EKU, and validity period, must be correctly configured for the system to successfully validate it. If any required component is missing, the certificate will fail validation and the connection will not be trusted.

-- 

## Stretch

In the real world, there was a certificate outage in 2020 that affected Microsoft Teams, where users were unable to access the application and received error messages. After learning more about PKI, I now understand that the outage was caused by an expired certificate that was supposed to be renewed but was not. The validity period was not properly managed, which led to the certificate expiring and services failing. This issue was resolved by reissuing a new certificate and restoring trust so systems could properly validate the secure connections again.

__

## Why This Matters

This matters because most PKI outages are caused by configuration and management mistakes, not broken cryptography. Being able to identify issues like expired certificates, missing intermediates, and incorrect configurations is a key skill for PKI and security professionals.
