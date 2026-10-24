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

> **Reference example.** The values below come from the example Metasploitable2 scan shown in the course kick-off / how-to material. They are **not** original results produced by this repository's author. Replace them with your own results once you run your own scan.

| Field | Reference example |
|---|---|
| **Policy** | Basic Network Scan |
| **Status** | Completed |
| **Severity Base** | CVSS v3.0 |
| **Scanner** | Local Scanner |
| **Hosts scanned** | 1 |
| **Duration** | ~12 minutes (09:59 → 10:11) |
| **Entries in the Vulnerabilities tab** | 70 |
| **Entries in the Remediations tab** | 3 |

### Severity Distribution

Nessus reports findings by severity: **Critical · High · Medium · Low · Info**. Host-level counts in the reference example:

| Severity | Count |
|---|---|
| Critical | 10 |
| High | 7 |
| Medium | 24 |
| Low | 9 |
| Info | 133 |

> *Info* items are informational findings (for example service detection), not vulnerabilities. Metasploitable2 is *intentionally* loaded with weaknesses, so a high number of Critical/High findings is expected for this training VM.

---

## Job 04 — Identifying Critical Vulnerabilities

> **Reference example** — top entries of the Vulnerabilities tab, sorted by severity. Plugin names that are cut off in the source screenshot are marked *(truncated in source)* and are intentionally not completed here.

| Severity | CVSS | Plugin name (as displayed) | Family | Count |
|---|---|---|---|---|
| Critical | 10.0* | UnrealIRCd Backdoor Detection | Backdoors | 1 |
| Critical | 10.0* | `VNC Serve…` *(truncated in source)* | Gain a shell remotely | 1 |
| Critical | 9.8 | `SSL Versio…` *(truncated in source)* | Service detection | 2 |
| Critical | 9.8 | `Bind Shell…` *(truncated in source)* | Backdoors | 1 |
| Critical (grouped) | n/a | `SSL (…` *(truncated in source)* | Gain a shell remotely | 3 |
| Mixed (grouped) | n/a | `Apac…` *(truncated in source)* | Web Servers | 4 |
| High | 7.5 | Samba Badlock Vulnerability | General | 1 |
| High | 7.5* | `rlogin Serv…` *(truncated in source)* | Service detection | 1 |

The UnrealIRCd entry also shows a VPR of 7.4 and an EPSS of 0.6495, which marks it as the finding with the highest estimated exploitation likelihood in this list.

### Types of Vulnerabilities Observed

- **Backdoors** — the UnrealIRCd and bind-shell findings. A backdoor gives remote access without valid credentials, which is why Nessus rates them at the top of the scale.
- **Remote shell / weak authentication** — a VNC finding in the *Gain a shell remotely* family, rated 10.0. This points to missing or weak authentication on a remote-access service.
- **Obsolete cryptographic protocols** — SSL-related findings. Old SSL versions have known weaknesses and should be replaced by TLS 1.2+.
- **Legacy remote services** — rlogin transmits data without encryption and relies on weak trust mechanisms.
- **Outdated software with known CVEs** — for example the Samba Badlock issue and the ISC BIND denial-of-service entry listed in the Remediations tab.

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
