# Claude-Opus-4.7-OWASP-LLM08

Empirical case-study disclosure of OWASP LLM08 (Excessive Agency) against Claude Opus 4.7 in Anthropic's sandbox executing production Fastly CDN changes. Coordinated disclosure to Anthropic + Fastly. The repo is a publication artifact, not a buildable project.

## Layout
```
README.md                              # disclosure overview + classification + contents map
REPORT.md                              # full disclosure (35-page A4 source)
REPORT.pdf                             # full disclosure (PDF, primary artifact)
NuClide-LLM08-Excessive-Agency.pdf     # supplementary primer on the LLM08 framing
nuclide_llm08_card.png                 # hero card image used in the README header
audit_log_api_response.json            # raw Fastly GET /events API capture (primary evidence)
screenshots/                           # 35-PNG visual evidence archive
  conversation/                        # 19 PNGs of the original Claude Opus 4.7 session
  audit_log_full/                      # 16 PNGs of the Fastly audit log expanded
LICENSE                                # CC-BY-4.0 (full text)
```

## Classification

- OWASP LLM Top 10 — **LLM08 Excessive Agency** (all three sub-types: Functionality, Permissions, Autonomy)
- CWE-441 (Confused Deputy), CWE-269 (Improper Privilege Management), CWE-223 (Omission of Security-Relevant Information), CWE-494 (Download Without Integrity Check), CWE-732 (Incorrect Permission Assignment)
- MITRE ATLAS AML.T0011 (User Execution); NIST AI 600-1 GAI 1.4; Simon Willison "Lethal Trifecta" (all three legs present)

## The actor split (the load-bearing finding)

When an LLM in a vendor sandbox executes production API changes against a SaaS authenticated as the human user, the receiving system's audit log attributes every change to the human account-holder. Recoverable only by inspecting the per-event `ip` field — which most SaaS UIs do not surface by default.

In this case:
- **LLM-in-Anthropic-sandbox**: GCP `35.223.241.4`
- **User-at-keyboard**: Google Fiber `136.37.103.3`

Same `account_id`, two different IPs, audit log only highlights the account_id.

## Claude Code Notes
- Read `REPORT.pdf` (or `REPORT.md`) for the full disclosure — that's the primary artifact
- `audit_log_api_response.json` is the raw evidence; `screenshots/` is the visual chain
- The README "Use with Claude Code" section has prompt templates for sub-type analysis, MCP detection-policy drafting, and reproduction-harness construction
- Coordinated-disclosure thread to Anthropic + Fastly ran in parallel; this repo is the public version
- Built with [Claude Code](https://claude.ai/code)
