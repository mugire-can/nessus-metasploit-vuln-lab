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

| Field | Example |
|---|---|
| **Policy** | Basic Network Scan |
| **Status** | Completed |
| **Severity Base** | CVSS v3.0 |
| **Scanner** | Local Scanner |
| **Elapsed** | ~12 minutes |

### Severity Distribution (Donut Chart)

Nessus reports findings by severity: **Critical · High · Medium · Low · Info**

Metasploitable2 is *intentionally* loaded with vulnerabilities, so expect a large number of findings across all severities (dozens of Critical/High findings is normal and expected for this specific training VM).

---

## Job 04 — Identifying Critical Vulnerabilities

> ✏️ Fill in this table with your **actual scan results**. Below is the structure to follow, based on commonly-found Metasploitable2 issues (do not copy values — replace with what your own scan reports).

| CVSS | Plugin Name | Family | Description (short) |
|---|---|---|---|
| `<score>` | `<e.g. UnrealIRCd backdoor>` | Backdoors | `<one-line summary>` |
| `<score>` | `<e.g. VNC Server — unauthenticated access>` | Gain a shell remotely | `<one-line summary>` |
| `<score>` | `<e.g. Samba Badlock>` | General | `<one-line summary>` |
| `<score>` | `<e.g. rlogin service>` | Service detection | `<one-line summary>` |

### Types of Vulnerabilities Typically Found on Metasploitable2

- **Backdoors** — e.g. a trojaned service binary allowing unauthenticated remote shell access
- **Weak/default credentials** — services with no authentication or default passwords
- **Outdated software** — unpatched daemons with known public CVEs
- **Insecure protocols** — deprecated SSL/TLS versions, unencrypted services (FTP, Telnet)
- **Misconfigurations** — services exposing more functionality than intended (e.g. NFS world-writable shares)

> ✏️ **Describe here, in your own words, the actual vulnerability types your scan uncovered** and why each is dangerous (e.g., "a Critical backdoor vulnerability allows unauthenticated remote code execution, meaning an attacker does not need any credentials to gain a shell").

---

## Exporting the Report

Nessus → scan → **Export** → choose format:
- `.nessus` (raw XML — **do not commit to the repo**, see `.gitignore`)
- PDF / HTML (readable report — **do not commit raw copies**, use sanitized summaries in `reports/`)
- CSV (useful for further processing, e.g. via the Talend ETL pipeline — see below)

---

## Optional — Data Pipeline for Reporting (Talend + SQL Server + Qlik)

For larger-scale or recurring scanning, this project also explored an automated pipeline:

```
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
