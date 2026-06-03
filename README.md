# BlackStack

[![CI](https://github.com/Qwkapital/blackstack-public/actions/workflows/ci.yml/badge.svg)](https://github.com/Qwkapital/blackstack-public/actions/workflows/ci.yml)
[![Agents](https://img.shields.io/badge/AI%20Agents-6-purple?style=flat-square)](#agent-roster)
[![MCP Servers](https://img.shields.io/badge/MCP%20Servers-45-blue?style=flat-square)](#layer-4-engineering)
[![Controls](https://img.shields.io/badge/Controls-233-green?style=flat-square)](#)
[![SSRN](https://img.shields.io/badge/SSRN-6791198-orange?style=flat-square)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6791198)
[![Website](https://img.shields.io/badge/Website-quantumwealthkapital.com-gold?style=flat-square)](https://quantumwealthkapital.com)



Autonomous technology platform serving QWK LLC and the Quantum Wealth Kapital ecosystem. Orchestrates a multi-agent swarm across 6 specialized agents (4 worker lanes + 2 control), 45 MCP servers (192+ tools), and a modular Bridge relay with deterministic routing, quality scoring, and human-in-the-loop risk gates.

**Repository**: `Qwkapital/blackstack` (private) | **Branch**: `main`

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for full system diagrams, planes, and data flows.

| Plane | Purpose | Key Components |
|-------|---------|----------------|
| Front Door | User intake | Open WebUI (`model=nexus`), Telegram approvals, CLI admin |
| Control | Orchestration | NEXUS-Core, Bridge v3.9 (2,685 lines, 20 lib modules) |
| Worker | Execution | Claude (builder/reviewer), Codex (failover), Gemini (researcher), Ghost (Ollama local) |
| Tool | Capabilities | 45 MCP servers, 192+ tools, 7 original n8n automation skills |
| State | Persistence | Redis (stigmergy), Qdrant (5 collections, BGE-M3 embeddings), Git |

## Policy Context

BlackStack v3.0 addresses the governance gap created when existing U.S. federal AI risk frameworks were applied to multi-agent autonomous systems deployed in regulated financial institutions.

| Framework | Issued | Relevance |
|-----------|--------|-----------|
| SR 11-7 | April 2011 | Model risk management -- predates autonomous AI by 15 years |
| OCC Bulletin 2026-13 | 2026 | AI risk in federal banking -- documents the governance gap (non-binding guidance, omits agentic implementation) |
| SR 26-2 | 2026 | AI model risk (Fed/OCC/FDIC joint) -- extends SR 11-7, does not address multi-agent orchestration |
| Treasury Report | December 2024 | Documents human capital gap in AI governance across FDIC-insured institutions |
| FSOC 2025 | 2025 | Flags AI governance as emerging systemic financial risk |

This gap affects all 4,336 FDIC-insured institutions. BlackStack v3.0 documents a practitioner implementation methodology, published in SSRN Abstract 6791198.

## Layer Structure

| Layer | Purpose | Key Contents |
|-------|---------|--------------|
| L1-principal | Principal layer | Personal workspace (private) |
| L2-corporate | Corporate (QWK/QKT) | Entity, strategy, compliance, decisions (DEC-001-021) |
| L3-governance | Tech governance | 103 policies, 6 agent specs, 26 audit modules |
| L4-engineering | Engineering | 45 MCP servers by department (core, data, services, platform, presentation) |
| L5-infrastructure | Infrastructure | Bridge runtime (20 lib modules), Docker, n8n v2.21.5, Redis, Qdrant |
| L6-operations | Operations | 105+ scripts, 10 hook guards, 36-entry crontab, Ed25519 audit signing |
| L7-marketing | Marketing | Brand, web (public_html), i18n (6 languages) |
| L8-research | R&D | Deep research synthesis, 52+ outputs, 4-layer memory architecture |

## Agent Roster

See [AGENTS.md](AGENTS.md) for full capabilities, routing, and governance model.

| Agent | Role | Model |
|-------|------|-------|
| NEXUS | Control plane -- routing, orchestration, policy enforcement | Orchestrator |
| SENTINEL | Cross-validation, fan-out, council for HIGH risk decisions | Verifier |
| Claude | Primary builder, reviewer, architect | claude-sonnet-4-6 / claude-opus-4-7 |
| Codex | Failover builder -- Thompson Sampling dynamic routing | OpenAI Codex CLI |
| Gemini | Researcher, SAGE judge, council tiebreaker | Gemini 2.5 Pro / Flash |
| Ghost | Local inference, privacy gate | Ollama qwen3:4b (RTX 2060) |

## Security & Governance

See [SECURITY.md](SECURITY.md) and [GOVERNANCE.md](GOVERNANCE.md).

- **53 mechanical controls** active: 10 hook guards (PreToolUse/PostToolUse) + 10-guard pre-commit
- **26 audit modules** across 5 AEGIS domains (integrity, compliance, observability, evidence, dispatch)
- **Ed25519 audit signing** -- tamper-evident session log chain
- **OWASP Agentic Top 10 (2025)**: 7/10 mitigated; 3 open (AA1 injection, AA7 SSRF, AA9 sandboxing)
- **Standards**: NIST AI RMF 1.0 (73%), ISO/IEC 42001:2023 (71%), NIST CSF 2.0, COBIT 2019, SOC 2 Type I
- **Google A2A Protocol v0.2** Phase 2 adopted (Phase 3 AgentCard endpoint pending)

## Automation

See [n8n/](n8n/) for the 7 original automation skills and workflows we built on top of n8n v2.21.5.

Key skills: Swarm Task Runner (Bridge dispatch), Research Sweep (daily cron), data pipeline, notification routing, audit automation, compliance workflow.

## MCP Servers

See [MCP_CATALOG.md](MCP_CATALOG.md) for full catalog (45 servers, 192+ tools, 5 departments).

Active highlights: `nexus-swarm`, `nexus-ops`, `docgen`, `knowledge-graph`, `finance-db`,
`compliance-engine`, `health-monitor`, `gmail-personal`, `playwright`, `firecrawl`, `tavily`,
`n8n-mcp`, `sequential-thinking`, `obsidian`, `mcp-observability`, `mcp-analytics`.

## Skills

See [SKILLS.md](SKILLS.md) for all 30 Claude Code skills deployed in the swarm (routing, research,
audit, document generation, compliance, infrastructure management, and more).

## Release History

| Version | Date | Audit Modules | Notes |
|---------|------|---------------|-------|
| v3.0 | May 2026 | 14 | Production deployment; SOC 2 / ISO 27001 / NIST CSF / COBIT 2019 validated |
| v3.9 | Current | 26 | Expanded coverage across all 8 architectural layers |

Production-deployed modules are anchored at v3.0 (14 modules). Active development continues at v3.9 (26 modules). See [CHANGELOG.md](CHANGELOG.md) for full version history and milestone timeline.

## Research

### AI Governance for FDIC-Insured Institutions: A Practitioner's Framework

Published May 2026 -- SSRN Financial Economics Network -- [Abstract 6791198](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6791198)

Documents the governance gap in U.S. federal AI risk frameworks for regulated financial institutions and presents a practitioner methodology validated in production. BlackStack v3.0 (14 modules, ISO 27001/SOC 2/NIST CSF/COBIT) is the reference implementation.

See [RESEARCH.md](RESEARCH.md) for full details, regulatory alignment table, and forthcoming work.

## Development Model

BlackStack is built and operated by a single principal engineer. The private repository (`Qwkapital/blackstack`) contains the full operational codebase. This public repository (`Qwkapital/blackstack-public`) contains architecture documentation, governance framework references, and public-facing specifications.

All modules are production-deployed and operationally validated before version tagging.
