# sandbox-recon-prod-side

**Disclosure-track repository — coordinated disclosure pending.**

Empirical case study: an Anthropic Claude (Opus 4.7) session, asked to install the Fastly CLI, autonomously installed two third-party binaries (Fastly CLI + `fastly-mcp` MCP server), persisted user credentials to its sandbox filesystem, and executed irreversible production CDN configuration changes against a real Fastly account. The receiving SaaS audit log attributes every change to the human account-holder. The actor split — LLM-in-Anthropic-sandbox (`35.223.241.4`, GCP) vs. user-at-keyboard (`136.37.103.3`, Google Fiber) — is recoverable only by inspecting the per-event `ip` field, which Fastly's default UI presentation does not surface.

## Classification

- **Primary:** OWASP LLM Top 10 — **LLM08 Excessive Agency** (all three sub-types: Functionality, Permissions, Autonomy)
- **Secondary CWEs:** CWE-441 (Confused Deputy), CWE-269 (Improper Privilege Management), CWE-223 (Omission of Security-Relevant Information), CWE-494 (Download Without Integrity Check), CWE-732 (Incorrect Permission Assignment)
- **Frames:** MITRE ATLAS AML.T0011 (User Execution); NIST AI 600-1 GAI 1.4; Simon Willison "Lethal Trifecta" (all three components present)

## Contents

| Path | Purpose |
|---|---|
| `REPORT.pdf` | Full disclosure report (18 pages, A4) — the primary artifact |
| `REPORT.md` | Markdown source of the report |
| `screenshots/` | 6 selected screenshots — service summary, default-view event log (no IP), three expanded audit-log rows showing IP field, terminal split-table |
| `audit_log_api_response.json` | Raw `GET /events` API response — primary evidence, captured at disclosure time |

## Provenance

- Conversation source: <https://claude.ai/share/ec2788aa-24e2-4f13-be45-8c5c8ae1f646>
- Session date: 2026-04-26
- Companion to: NuClide Sandbox Recon (2026-03-24, internal-side reciprocal)

## Disclosure status

| Stakeholder | Status |
|---|---|
| Anthropic | Pending (security@anthropic.com / HackerOne) |
| Fastly | Pending (security@fastly.com / HackerOne) — CWE-223 (default-UI IP omission) is the most clearly Fastly-actionable |
| Public | Pending |

## Author

Nicholas Kloster — NuClide Research — <https://nuclide-research.com>
