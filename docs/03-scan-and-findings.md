# 03 — Scan Configuration & Findings

## Job 04 — Running the Scan

### Step 1 — Create a New Scan

In the Nessus web UI (`https://<debian-vm-ip>:8834`):

1. **Scans** → **New Scan**
2. Choose a scan template under **Vulnerabilities**:

| Template | Use Case |
|---|---|
| **Basic Network Scan** | Full system scan suitable for any host — used in this lab |
| **Advanced Scan** | Fully customizable, no built-in recommendations |
| **Credentialed Patch Audit** | Authenticate to hosts, enumerate missing updates |
| **Web Application Tests** | Scan for published/unknown web vulnerabilities |
| **Malware Scan** | Scan for malware on Windows/Unix systems |

For this lab: **Basic Network Scan**.

### Step 2 — Configure Scan Settings

| Field | Value |
|---|---|
| **Name** | `Scan de la VM "Metasploitable 2"` |
| **Folder** | My Scans |
| **Targets** | `<metasploitable2-ip>` |

Leave **Credentials** empty for an unauthenticated scan (or configure SSH credentials `msfadmin`/`msfadmin` for a credentialed scan — more thorough, fewer false positives).

### Step 3 — Launch

Click **Save**, then **Launch** ▶️.  
A Basic Network Scan against Metasploitable2 typically takes **10–15 minutes**.

---

## Reading the Results

### Scan Summary

| Field | Value |
|---|---|
| **Policy** | Basic Network Scan |
| **Status** | Completed |
| **Severity Base** | CVSS v3.0 |
| **Scanner** | Local Scanner |
| **Hosts scanned** | 1 |

### Severity Distribution

| Severity | Count |
|---|---|
| Critical | 10 |
| High | 7 |
| Medium | 27 |
| Low | 8 |
| Info | 141 |

> *Info* items are informational findings (for example service detection), not vulnerabilities. Metasploitable2 is *intentionally* loaded with weaknesses, so a high number of Critical/High findings is expected for this training VM.

---

## Job 04 — Identifying Critical Vulnerabilities

Top findings from the scan, sorted by CVSS score:

| CVSS | Plugin Name | Family | Count |
|---|---|---|---|
| 10.0 | Canonical Ubuntu Linux SEoL (8.04.x) | General / Service detection | 1 |
| 10.0* | VNC Server 'password' Password | Gain a shell remotely | 1 |
| 10.0* | Debian OpenSSH/OpenSSL Package Random Number Generator Weakness (SSL check) | Gain a shell remotely | 2 |
| 10.0* | Debian OpenSSH/OpenSSL Package Random Number Generator Weakness | Gain a shell remotely | 1 |
| 10.0 | Apache Tomcat SEoL (<= 5.5.x) | Web Servers | 1 |
| 9.8 | SSL Version 2 and 3 Protocol Detection | Service detection | 2 |
| 9.8 | Bind Shell Backdoor Detection | Backdoors | 1 |
| 7.5 | Samba Badlock Vulnerability | General | 1 |

> `*` indicates a CVSS score flagged by Nessus as modified from the plugin's base score (e.g., adjusted for context).

### Types of Vulnerabilities Found

- **End-of-life software (SEoL)** — both the base OS (Ubuntu 8.04.x) and Apache Tomcat (<= 5.5.x) are past end-of-life, meaning they no longer receive security patches from their vendors at all. This is one of the most severe classes of finding: *any* future vulnerability in these components will never be fixed.
- **Weak/predictable authentication** — the VNC server accepts a password of `password`, allowing trivial unauthenticated remote desktop access.
- **Cryptographic weakness (Debian OpenSSL bug)** — the Debian OpenSSH/OpenSSL packages are affected by the well-known 2008 Debian weak-key vulnerability, which drastically reduces the entropy of generated SSH keys, making them predictable/crackable.
- **Backdoors** — a Bind Shell Backdoor is detected directly listening on the host, meaning an attacker can connect and get a shell with no authentication at all.
- **Obsolete cryptographic protocols** — SSLv2/SSLv3 are enabled, both of which have known, exploitable weaknesses (e.g., POODLE) and should be disabled in favor of TLS 1.2+.
- **Outdated software with known CVEs** — the Samba Badlock vulnerability affects the SMB implementation and is fixed in later Samba versions.

---

## Exporting the Report

Nessus → scan → **Export** → choose format:

- `.nessus` (raw XML — **do not commit to the repo**, see `.gitignore`)
- PDF / HTML (readable report — **do not commit raw copies**, use sanitized summaries in `reports/`)
- CSV (useful for further processing, e.g. via the Talend ETL pipeline — see below)

---

## Optional — Data Pipeline for Reporting (Talend + SQL Server + Qlik)

For larger-scale or recurring scanning, this project also explored an automated pipeline:

```text
Nessus REST API
   │  POST /session         (authenticate, get token)
   │  POST /scans/{id}/export (request XML export)
   │  GET  /scans/{id}/export/{file}/download  (download XML)
   ▼
Talend Open Studio (ETL)
   │  tFileInputXML  — parse ReportHost / ReportItem nodes via XPath
   │  tJavaRow       — normalize fields (timestamp conversion, etc.)
   ▼
Microsoft SQL Server  (normalized findings table)
   ▼
Qlik  (interactive dashboards / KPIs)
```

Key XPath queries used to extract structured data from the raw `.nessus` XML:

- Host loop: `/NessusClientData_v2/Report/ReportHost`
- Finding loop: `/NessusClientData_v2/Report/ReportHost/ReportItem`
- Fields extracted: `scan`, `host`, `pluginID`, `pluginName`, `pluginFamily`, `protocol`, `port`

This converts Nessus's verbose raw XML reports into clean, queryable, normalized data — enabling trend analysis and dashboarding over time instead of one-off PDF reports.

> Implementation details (Talend job screenshots, SQL schema) are intentionally not included raw in this repo; see `reports/README.md` for sanitized summaries only.

---

*Previous: [02 — Lab Setup](./02-lab-setup.md) | Next: [04 — Exploitation Notes](./04-exploitation-notes.md)*
