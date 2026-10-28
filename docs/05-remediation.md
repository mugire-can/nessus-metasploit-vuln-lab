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

| Priority | CVSS | Finding | Recommended Fix |
|---|---|---|---|
| 1 | 10.0 | Canonical Ubuntu Linux SEoL (8.04.x) | Upgrade the OS to a currently-supported Ubuntu LTS release; an end-of-life OS cannot be meaningfully "patched" |
| 2 | 10.0* | VNC Server 'password' Password | Change the VNC password immediately, enforce a strong password policy, or disable/restrict VNC access via firewall |
| 3 | 10.0* | Debian OpenSSH/OpenSSL RNG Weakness (x2 findings) | Regenerate all SSH host/user keys using a patched OpenSSL version; this is a historical, well-documented Debian packaging bug (2008) |
| 4 | 10.0 | Apache Tomcat SEoL (<= 5.5.x) | Upgrade Tomcat to a currently-supported major version |
| 5 | 9.8 | SSL Version 2 and 3 Protocol Detection | Disable SSLv2/SSLv3 on all affected services, require TLS 1.2 or later |
| 6 | 9.8 | Bind Shell Backdoor Detection | Treat the host as compromised: identify and remove the backdoor process/binary, investigate how it was introduced, rebuild from a trusted image if in a real environment |
| 7 | 7.5 | Samba Badlock Vulnerability | Upgrade Samba to a patched version (4.2.11 / 4.3.8 / 4.4.2 or later) |

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
