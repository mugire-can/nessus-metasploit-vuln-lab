# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- `.github/` issue templates (documentation improvement, question)
- `.github/PULL_REQUEST_TEMPLATE.md` with a data-sanitization checklist
- Markdown lint CI workflow (`.github/workflows/markdown-lint.yml`)
- `assets/images/README.md` screenshot guidelines and sanitization checklist

## [0.2.0] - Documentation Phase

### Added

- `docs/01-nessus-overview.md` — vulnerability scanning concepts, tool comparison, CVE/CVSS reference
- `docs/02-lab-setup.md` — Metasploitable2 and Nessus VM setup instructions
- `docs/03-scan-and-findings.md` — scan configuration, findings analysis, optional Talend/SQL Server/Qlik pipeline notes
- `docs/04-exploitation-notes.md` — Nessus-to-Metasploit exploitation methodology
- `docs/05-remediation.md` — remediation guidance and security posture recommendations
- `reports/README.md` — rules for sanitized findings summaries

## [0.1.0] - Initial Structure

### Added

- Initial repository structure
- `README.md` with project overview, architecture diagram, and job breakdown
- `LICENSE` (MIT)
- `.gitignore` excluding raw scan exports, PDFs, and credential-bearing files