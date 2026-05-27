# BlackStack

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

## Layer Structure

| Layer | Purpose | Key Contents |
|-------|---------|--------------|
| L1-principal | Principal layer | Personal domains: health, education, finance |
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
| NEXUS | Control plane — routing, orchestration, policy enforcement | Orchestrator |
| SENTINEL | Cross-validation, fan-out, council for HIGH risk decisions | Verifier |
| Claude | Primary builder, reviewer, architect | claude-sonnet-4-6 / claude-opus-4-7 |
| Codex | Failover builder — Thompson Sampling dynamic routing | OpenAI Codex CLI |
| Gemini | Researcher, SAGE judge, council tiebreaker | Gemini 2.5 Pro / Flash |
| Ghost | Local inference, privacy gate | Ollama qwen3:4b (RTX 2060) |

## Security & Governance

See [SECURITY.md](SECURITY.md) and [GOVERNANCE.md](GOVERNANCE.md).

- **53 mechanical controls** active: 10 hook guards (PreToolUse/PostToolUse) + 10-guard pre-commit
- **26 audit modules** across 5 AEGIS domains (integrity, compliance, observability, evidence, dispatch)
- **Ed25519 audit signing** — tamper-evident session log chain
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

Production-deployed modules are anchored at v3.0 (14 modules). Active development continues at v3.9 (26 modules).

## Development Model

BlackStack is built and operated by a single principal engineer. The private repository (`Qwkapital/blackstack`) contains the full operational codebase. This public repository (`Qwkapital/blackstack-public`) contains architecture documentation, governance framework references, and public-facing specifications.

All modules are production-deployed and operationally validated before version tagging.
