# Reports — Sanitized Findings Only

This folder is intended for **sanitized, anonymized summaries** of scan results — not raw exports.

## What belongs here

- Markdown/CSV summaries with **no real IP addresses, hostnames, or credentials**
- Severity breakdowns (counts per Critical/High/Medium/Low/Info)
- High-level write-ups referencing finding names/CVEs (public identifiers only)

## What does NOT belong here (and is excluded via `.gitignore`)

- Raw `.nessus` / XML exports
- Raw PDF/HTML scan reports exported directly from Nessus
- Any file containing real lab IPs, session tokens, or credentials

## Example structure

```text
reports/
├── README.md              (this file)
└── scan-summary-2026-XX-XX.md   (sanitized summary, add as needed)
```
