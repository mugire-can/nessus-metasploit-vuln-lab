# 05 — Remediation (Job 07)

## Remediation Principles

Every vulnerability identified should map to a concrete, actionable fix. Nessus itself provides a **Remediations** tab that aggregates recommended actions across all findings in a scan, typically along these lines:

| Finding Type | Typical Remediation |
|---|---|
| Outdated software / missing patches | Upgrade to the latest stable/patched version |
| Weak or default credentials | Enforce strong password policy, disable default accounts |
| Insecure protocol (e.g. deprecated TLS, Telnet, FTP) | Disable the protocol, migrate to its secure equivalent (TLS 1.2+, SSH, SFTP) |
| Backdoor / trojaned service | Remove and reinstall from a verified, checksum-validated source |
| Misconfigured service (e.g. SMB signing disabled) | Apply secure configuration baseline (GPO / hardening guide) |
| Unnecessary exposed service | Disable the service or restrict access via firewall rules |

## Remediation Table

> **Reference example**, based on the course's example scan of Metasploitable2. The *Source* column states where each fix comes from: the Nessus **Remediations** tab, or general hardening guidance added for context.

| Priority | Severity | Finding | Recommended fix | Source |
|---|---|---|---|---|
| 1 | Critical | UnrealIRCd Backdoor Detection | Re-download the software, verify it with the published MD5/SHA1 checksums, and re-install it | Nessus Remediations tab |
| 2 | Critical | Bind shell backdoor *(name truncated in source)* | Treat the host as compromised: close the listener, investigate, and rebuild from a trusted image | General guidance |
| 3 | Critical | VNC server *(name truncated in source)* | Enforce strong authentication, restrict access with firewall rules, or tunnel it through SSH | General guidance |
| 4 | Critical | SSL v2/v3 protocol detection *(name truncated in source)* | Disable obsolete SSL versions and require TLS 1.2 or later | General guidance |
| 5 | High | Samba Badlock Vulnerability | Upgrade Samba to version 4.2.11 / 4.3.8 / 4.4.2 or later | Nessus Remediations tab |
| 6 | High | rlogin service *(name truncated in source)* | Disable rlogin and use SSH instead | General guidance |
| 7 | n/a | ISC BIND denial of service (3 vulnerabilities) | Upgrade BIND to 9.11.22, 9.16.6 or 9.17.4 or later | Nessus Remediations tab |

### Nessus Remediations tab (reference example)

The scan produced **3 consolidated actions** for the single scanned host:

| Action | Vulnerabilities fixed | Hosts |
|---|---|---|
| ISC BIND 9.x DoS: upgrade to BIND 9.11.22, 9.16.6, 9.17.4 or later | 3 | 1 |
| Samba Badlock: upgrade to Samba 4.2.11 / 4.3.8 / 4.4.2 or later | 1 | 1 |
| UnrealIRCd Backdoor Detection: re-download, verify checksums, re-install | 0 | 1 |

> The *Vulnerabilities fixed* column is shown as displayed by Nessus. The backdoor action lists 0 there even though it is a Critical finding, so use it as an indicator of consolidated plugin counts rather than risk.

---

## Why Regular Updates & Security Audits Matter

- **Patch management**: most Critical/High findings trace back to software that is simply out of date. A disciplined patch cadence eliminates the majority of exploitable vulnerabilities before they're ever found by an attacker.
- **Regular scanning**: a single scan is a snapshot in time. New CVEs are published daily — periodic (e.g. weekly/monthly) scanning catches newly disclosed vulnerabilities in already-deployed software.
- **Credentialed scans**: provide far deeper visibility (missing patches, local misconfigurations) than unauthenticated scans, and should be the standard for internal assets.
- **Defense in depth**: scanning + patching alone isn't enough — network segmentation, least-privilege access, and monitoring reduce the blast radius when a vulnerability *is* eventually exploited.
- **Validate, don't just scan**: as demonstrated in this project, combining a scanner (Nessus) with exploitation validation (Metasploit) in a controlled environment helps prioritize *real, demonstrated* risk over purely theoretical CVSS scores.

---

## Reference — Statistics on Attack Vectors

(For context — general industry figures, not specific to this lab)

| Attack Mode | Observed Share |
|---|---|
| Phishing | ~60% |
| Exploitation of a vulnerability | ~43% |
| DDoS attacks | ~34% |
| CEO fraud / business email compromise | ~28% |

| Root Cause of Breaches | Share |
|---|---|
| Unapproved application deployment | ~35% |
| Residual/unpatched vulnerabilities | ~34% |
| Negligence or configuration error | ~33% |

These figures reinforce why **vulnerability management** (this project's focus) is consistently one of the top risk-reduction activities an organization can invest in.

---

*Previous: [04 — Exploitation Notes](./04-exploitation-notes.md) | Back to: [README](../README.md)*
