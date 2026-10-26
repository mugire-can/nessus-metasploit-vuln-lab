# Nessus & Metasploit Vulnerability Assessment Lab

[![Markdown Lint](https://github.com/mugire-can/nessus-metasploit-vuln-lab/actions/workflows/markdown-lint.yml/badge.svg)](https://github.com/mugire-can/nessus-metasploit-vuln-lab/actions/workflows/markdown-lint.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Hands-on cybersecurity training project: scanning for vulnerabilities with **Tenable Nessus** and validating exploitability with **Metasploit** against **Metasploitable2**, inside an isolated, legally-authorized lab environment.

This repository documents the methodology, setup, findings analysis, and remediation process carried out as part of a vulnerability management training module.

---

## ⚠️ Legal & Ethical Disclaimer

All scanning and exploitation activities documented here were performed **exclusively** against:

- A **deliberately vulnerable, purpose-built virtual machine** (Metasploitable2), designed by Rapid7 for security training.
- An **isolated lab network**, never exposed to the internet or to systems outside the scope of this exercise.

No real, third-party, or production system was scanned or targeted. Using the tools and techniques described here against any system without explicit, written authorization is **illegal** and a violation of professional ethics. This repository is for **educational purposes only**.

---

## 🎯 Objectives

- Understand what a vulnerability scan is and how it works
- Install, configure, and operate Nessus
- Build a controlled, disposable attack-lab (Metasploitable2 + Nessus scanner)
- Run vulnerability scans, interpret CVSS-based severity, and identify critical findings
- Map a detected vulnerability to a corresponding Metasploit exploit module
- Perform controlled exploitation and basic post-exploitation in the lab
- Propose remediation actions for discovered vulnerabilities
- (Extension) Pipe scan results through an ETL process (Talend) into a database for reporting/visualization (Qlik)

## 🧰 Tools Used

| Tool | Role |
|---|---|
| [Tenable Nessus](https://www.tenable.com/products/nessus) | Vulnerability scanner |
| [Metasploit Framework](https://www.metasploit.com/) | Exploitation framework |
| [Metasploitable2](https://docs.rapid7.com/metasploit/metasploitable-2/) | Intentionally vulnerable target VM |
| Debian (VM) | Nessus scanner host |
| Talend Open Studio | ETL — extracting Nessus scan results into a database |
| Microsoft SQL Server | Storage for normalized findings |
| Qlik | Dashboarding / visualization of vulnerability data |

## 🏗️ Lab Architecture

```text
            ┌────────────────────┐
            │   Attacker host     │
            │ (Kali / Metasploit) │
            └─────────┬───────────┘
                      │ exploit
                      ▼
┌──────────────────┐     scan     ┌───────────────────────┐
│  Nessus Scanner   │ ───────────► │   Metasploitable2      │
│ (Debian VM)        │ ◄─────────── │ (isolated target VM)   │
└──────────────────┘   report      └───────────────────────┘
```

All components run in an isolated host-only / NAT network segment with no route to production or public networks.

## 📁 Repository Structure

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/              # Issue templates (docs improvement, question)
│   ├── workflows/
│   │   └── markdown-lint.yml        # CI: markdown linting on push / PR
│   └── PULL_REQUEST_TEMPLATE.md     # PR checklist incl. data-sanitization checks
├── docs/
│   ├── 01-nessus-overview.md        # What is a vulnerability scan / Nessus basics
│   ├── 02-lab-setup.md              # VM provisioning, Nessus install & config
│   ├── 03-scan-and-findings.md      # Scan configuration, results summary, severity breakdown
│   ├── 04-exploitation-notes.md     # Vulnerability-to-exploit mapping, methodology (no live payload data)
│   └── 05-remediation.md            # Fix recommendations, patching/hardening notes
├── assets/
│   └── images/                      # Anonymized diagrams / screenshots (+ guidelines)
├── reports/
│   └── README.md                    # Sanitized findings summaries only (no raw exports)
├── .editorconfig                    # Editor defaults (UTF-8, LF, final newline)
├── .gitignore
├── .markdownlint.json               # Markdown lint rules used by CI
├── CHANGELOG.md                     # Version history
└── LICENSE
```

> Raw `.nessus` / XML exports, full PDF scan reports, and any file containing real host IPs, credentials, or scan tokens are intentionally **excluded** from this repository (see `.gitignore`). Only sanitized, anonymized summaries are published.

## 📌 Project Tasks (Jobs)

| Job | Description |
|---|---|
| 01 | Document how Nessus works |
| 02 | Deploy the Metasploitable2 target VM |
| 03 | Deploy a Debian VM and install/configure Nessus |
| 04 | Run a basic scan, analyze results, identify critical vulnerabilities |
| 05 | Map a Nessus finding to a Metasploit exploit module and configure it |
| 06 | Exploit a vulnerability in the lab, obtain access, basic post-exploitation |
| 07 | Propose remediation actions for the findings |

## 🛠️ Skills Demonstrated

- Administering and securing network infrastructure
- Administering and securing system infrastructure
- Detecting and handling security incidents
- Vulnerability management lifecycle (scan → analyze → remediate)

## 📚 References

- [Tenable Nessus Documentation](https://docs.tenable.com/nessus/)
- [Metasploit Framework Documentation](https://docs.metasploit.com/)
- [Rapid7 — Metasploitable2](https://docs.rapid7.com/metasploit/metasploitable-2/)

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.