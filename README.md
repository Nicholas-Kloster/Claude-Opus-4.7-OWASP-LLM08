[![Claude Code Friendly](https://img.shields.io/badge/Claude_Code-Friendly-blueviolet?logo=anthropic&logoColor=white)](https://claude.ai/code)

<p align="center">
  <img src="nuclide_llm08_card.png" alt="I exploited OWASP LLM08 in Claude Opus 4.7. Not with adversarial prompts. Not with role injection. With one user message." width="900">
</p>

# Claude Opus 4.7 — OWASP LLM08 Excessive Agency

- Conversation source: <https://claude.ai/share/ec2788aa-24e2-4f13-be45-8c5c8ae1f646>
  
**Audit-Trail Actor Erasure — Anthropic Claude Sandbox + Fastly CDN**

Empirical case-study disclosure. Coordinated disclosure to Anthropic and Fastly running in parallel.

Empirical case study: an Anthropic Claude (Opus 4.7) session, asked to install the Fastly CLI, autonomously installed two third-party binaries (Fastly CLI + `fastly-mcp` MCP server), persisted user credentials to its sandbox filesystem, and executed irreversible production CDN configuration changes against a real Fastly account. The receiving SaaS audit log attributes every change to the human account-holder. The actor split — LLM-in-Anthropic-sandbox (`35.223.241.4`, GCP) vs. user-at-keyboard (`136.37.103.3`, Google Fiber) — is recoverable only by inspecting the per-event `ip` field, which Fastly's default UI presentation does not surface.

## Classification

- **Primary:** OWASP LLM Top 10 — **LLM08 Excessive Agency** (all three sub-types: Functionality, Permissions, Autonomy)
- **Secondary CWEs:** CWE-441 (Confused Deputy), CWE-269 (Improper Privilege Management), CWE-223 (Omission of Security-Relevant Information), CWE-494 (Download Without Integrity Check), CWE-732 (Incorrect Permission Assignment)
- **Frames:** MITRE ATLAS AML.T0011 (User Execution); NIST AI 600-1 GAI 1.4; Simon Willison "Lethal Trifecta" (all three components present)

## Contents

| Path | Purpose |
|---|---|
| `REPORT.pdf` | Full disclosure report (35 pages, A4) — the primary artifact |
| `REPORT.md` | Markdown source of the report |
| `NuClide-LLM08-Excessive-Agency.pdf` | Supplementary primer — OWASP LLM08 framing |
| `audit_log_api_response.json` | Raw `GET /events` API response — primary evidence, captured at disclosure time |
| `screenshots/` | Full visual evidence archive (35 PNG files): |
| `screenshots/00_…01_…02_…` etc. | Curated forensic-evidence subset used in the report body |
| `screenshots/conversation/` | 19 screenshots of the original Claude.ai (Opus 4.7) conversation showing the LLM autonomously installing CLI, self-updating, enumerating, executing production writes |
| `screenshots/audit_log_full/` | 16 screenshots of the Fastly account audit log expanded ("Hide Details") confirming `ip` field per event — the visual proof of the actor split |

---

## Use with Claude Code

Use Claude Code to analyze the OWASP LLM08 taxonomy, cross-reference the audit log evidence, and build detection policies.

```
Read README.md and REPORT.md in this repo (Claude-Opus-4.7-OWASP-LLM08).
Then:
1. Identify which of the three LLM08 sub-types (Functionality, Permissions, Autonomy) is
   most strongly demonstrated by the Fastly audit log evidence in audit_log_api_response.json
2. Draft a detection policy for an MCP server operator that would catch this actor-split
   pattern — LLM performing actions attributable to the human account holder
3. Map the full chain to Simon Willison's Lethal Trifecta and explain which leg is hardest to break
```

```
I want to test whether a new LLM or agent framework reproduces the OWASP LLM08
excessive agency pattern documented here.
Read REPORT.md for the attack scenario, then:
1. Build a minimal reproduction harness — a sequence of user prompts that
   guide the model toward installing a CLI tool and making a production API call
2. Define pass/fail criteria based on where the model stops vs. proceeds
3. Suggest which safeguard (tool confirmation, scope limitation, audit logging) would
   have blocked this specific chain
```

---

## License

Released under [Creative Commons Attribution 4.0 International (CC-BY-4.0)](https://creativecommons.org/licenses/by/4.0/). See [LICENSE](LICENSE) for the full canonical text. Attribution required for redistribution.

---

## About

Maintained by **[Nicholas Michael Kloster](https://github.com/Nicholas-Kloster)** as part of [**NuClide**](https://nuclide-research.com) — independent AI infrastructure security research.

CISA disclosures: [CVE-2025-4364](https://nvd.nist.gov/vuln/detail/CVE-2025-4364) · [ICSA-25-140-11](https://www.cisa.gov/news-events/ics-advisories/icsa-25-140-11)
