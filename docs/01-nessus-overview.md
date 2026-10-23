# 01 — Nessus & Vulnerability Scanning Overview

## What is a Vulnerability Scan?

A vulnerability scan is an **automated analysis** that searches for security flaws in a system (server, PC, network, web application).

How it works:

- Probes **ports and services**, checks **software versions**
- Compares findings against a **known vulnerability database (CVE)**
- Flags issues: **outdated software**, **weak passwords**, **misconfigured services**
- Produces a **report** with severity ratings from Low → Critical (based on CVSS scoring)

---

## Main Vulnerability Scanning Tools

| Tool | Type | Key Characteristic |
|---|---|---|
| **Nessus** (Tenable) | Commercial / Trial | Most widely used, very rich plugin database, clear reports |
| **OpenVAS** (Greenbone) | Open-source | Free alternative to Nessus, more complex to set up and maintain |
| **QualysGuard** | Cloud SaaS | Enterprise-grade, used for compliance audits (ISO, PCI-DSS, GDPR) |
| **MD Vulnerability Mgmt** | Microsoft / Cloud | Integrated with Microsoft 365 & Defender, ideal for Windows ecosystems |
| **Acunetix** | Commercial | Specialized in web application scanning (SQLi, XSS, CSRF — OWASP) |

---

## Nessus

**Tenable Nessus** is one of the most widely known and used vulnerability scanners in the world.  
It offers a very rich database and clear reports for all types of infrastructure.

### Key Features

- Scans hosts, ports, services, and running software
- Identifies missing patches, weak configurations, default credentials
- Severity based on **CVSS v3.0** (Critical / High / Medium / Low / Info)
- Supports **credentialed scans** (deeper visibility, fewer false positives)
- Provides **remediations** tab with actionable fix suggestions
- Exportable reports (PDF, HTML, CSV, XML / `.nessus`)

### Licensing

Nessus is a paid product, but offers:

- **7-day free trial** (Nessus Professional / Expert)
- **Nessus Essentials** — free for up to 16 IPs (personal / training use)

Register at: [tenable.com/products/nessus](https://www.tenable.com/products/nessus)

---

## Metasploit Framework

**Metasploit** is an exploitation framework used to test and exploit vulnerabilities on systems.

- Contains a massive library of **ready-to-use exploits**
- Allows simulating an attack after a scan (e.g., from Nessus findings)
- Used by pentesters to verify whether a detected vulnerability is **actually exploitable**
- Maintained by **Rapid7**

> In this lab, Metasploit is used **only against Metasploitable2** — never against any real system.

---

## Metasploitable2

**Metasploitable2** is an intentionally vulnerable virtual machine created by Rapid7 for testing exploits.

Purpose:

- Serves as a **safe training ground** for learning pentesting and security
- Contains many **well-known vulnerabilities** (misconfigured services, outdated software)
- Allows practicing with Nessus, Metasploit, Nmap, etc. **without any legal risk**

Default credentials: `msfadmin` / `msfadmin`

> Warning shown at boot: *"Never expose this VM to an untrusted network!"*

---

## Lab Architecture

```text
                    ┌──────────────────────────┐
                    │  Metasploit (attacker VM) │
                    └────────────┬─────────────┘
                                 │ exploit
            ┌────────────────────▼──────────────────────┐
            │         Metasploitable2 — Target VM        │
            │              (Sandbox / Lab)               │
            └───────────────────────────────────────────┘
                                 ▲
                                 │ scan
                    ┌────────────┴─────────────┐
                    │  Nessus (scanner VM)      │
                    │  + reports + remediation  │
                    └──────────────────────────┘
```

All communication stays within the isolated lab network — no public internet exposure.

---

## CVE & CVSS — Quick Reference

| Term | Definition |
|---|---|
| **CVE** | Common Vulnerabilities and Exposures — unique ID for a known vulnerability (e.g., `CVE-2021-44228`) |
| **CVSS** | Common Vulnerability Scoring System — severity score from 0 to 10 |
| **Critical** | CVSS 9.0–10.0 — requires immediate action |
| **High** | CVSS 7.0–8.9 |
| **Medium** | CVSS 4.0–6.9 |
| **Low** | CVSS 0.1–3.9 |
| **Info** | Not a vulnerability — informational finding only |

---

*Next: [02 — Lab Setup](./02-lab-setup.md)*