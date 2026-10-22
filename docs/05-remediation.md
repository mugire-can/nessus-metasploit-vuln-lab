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

## ✏️ Remediation Table (fill in with your actual findings)

| Severity | Finding | Recommended Fix | Owner / Priority |
|---|---|---|---|
| Critical | `<finding>` | `<fix>` | `<immediate>` |
| High | `<finding>` | `<fix>` | `<short-term>` |
| Medium | `<finding>` | `<fix>` | `<scheduled>` |

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
