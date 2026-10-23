# Images — Guidelines

This folder holds **diagrams and anonymized screenshots** referenced from the `docs/` files.

## ✅ What to include

| File (suggested name) | Content |
|---|---|
| `architecture-diagram.png` | Lab network diagram (Nessus VM ↔ Metasploitable2 ↔ Attacker VM) |
| `nessus-scan-summary.png` | Nessus scan results screen (hosts tab) — **redact/blur the real IP** |
| `nessus-vuln-list.png` | Vulnerabilities tab with CVSS severity breakdown |
| `nessus-remediation-tab.png` | Remediations tab |
| `metasploit-exploit-search.png` | `search <keyword>` output in `msfconsole` |
| `metasploit-session.png` | Successful session/shell screenshot — **redact any real IP/hostname** |
| `talend-job-overview.png` | (optional) Talend ETL job diagram, if the pipeline extension is documented |
| `qlik-dashboard.png` | (optional) Qlik dashboard screenshot, if included |

## 🔒 Before committing any screenshot — sanitization checklist

- [ ] No real/public IP addresses visible (blur or replace with `10.x.x.x` placeholders)
- [ ] No hostnames that reveal organizational information
- [ ] No credentials, tokens, or API keys visible in terminal/browser screenshots
- [ ] No personal information (usernames tied to real identities, email addresses, etc.)
- [ ] File size kept reasonable (compress PNGs — e.g. via `pngquant` or export at a sane resolution)

## Referencing images in docs

```markdown
![Lab Architecture](../assets/images/architecture-diagram.png)
```

## Formats

- Prefer **PNG** for screenshots (lossless, clean text rendering)
- Prefer **SVG** for diagrams drawn from scratch, where possible
- Avoid `.psd`, `.ai`, or other large/unreadable source files in the repo