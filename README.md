# Blackstack

Autonomous technology platform serving QWK LLC and the Quantum Wealth Kapital ecosystem. Orchestrates a multi-agent swarm (16 agents, 40 MCP servers, 192+ tools) through a modular Bridge relay with deterministic routing, quality scoring, and human-in-the-loop risk gates.

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for full system diagrams, planes, and data flows.

| Plane | Purpose | Key Components |
| ----- | ------- | -------------- |
| Front Door | User intake | Open WebUI (`model=nexus`), Telegram approvals, CLI admin |
| Control | Orchestration | NEXUS-Core, Bridge v3.9, DAG engine |
| Worker | Execution | FORGE (Codex), ATLAS (Gemini), ORACLE (Aider), BOLT (Goose), GHOST (Ollama) |
| Tool | Capabilities | 40 MCP servers, n8n workflows, OpenFang |
| State | Persistence | Redis stigmergy, filesystem, Git |

## Layer Structure

| Layer | Purpose | Key Contents |
| ----- | ------- | ------------ |
| L1-principal | Personal | Health, finance, education, immigration |
| L2-corporate | Corporate | Entity, strategy, compliance, decisions (DEC-001-019) |
| L3-governance | Tech governance | 93 policies, 41 specs, 15 audit modules, agent specs |
| L4-engineering | Engineering | 40 MCP servers by department (core, data, services, platform) |
| L5-infrastructure | Infrastructure | Bridge runtime (20 lib modules), Docker, n8n, LiteLLM, Redis |
| L6-operations | Operations | 106 scripts, hooks, cron (33 entries), monitoring |
| L7-marketing | Marketing | Brand, web (public_html), i18n (6 languages) |
| L8-research | R&D | Sandbox, experiments, 68 research outputs |

## Quick Start

```bash
# Prerequisites: Node 24+, Python 3.12+, Redis, Docker
export BLACKSTACK_ROOT=~/blackstack

# Bootstrap environment
bash L6-operations/scripts/bootstrap.sh

# Start core services
bash L5-infrastructure/runtime/scripts/bridge-start.sh   # Bridge :5679
docker compose -f L5-infrastructure/runtime/compose/docker-compose.yml up -d

# Health check
bash L6-operations/scripts/health.sh

# Run tests
pytest tests/ -q && npx vitest run
```

## Services

| Service | Port | Status |
| ------- | ---- | ------ |
| Bridge relay | 5679 | Active |
| n8n | 5678 | Docker |
| Redis | 6379 | Docker |
| OpenFang | 50051 | Active |
| LiteLLM proxy | 4000 | Docker |
| Grafana | 3100 | Docker |
| Open WebUI | 3000 | Docker |

## Governance

- **Autonomy Framework**: 5 tiers (A0 full-auto to A4 CEO-only) + S-tiers for inter-agent delegation
- **Routing Policy**: Deterministic decision tree, anti-bias (cheapest adequate provider wins)
- **Quality**: SENTINEL fan-out for HIGH risk, PRSA protocol for multi-approach decisions
- **Audit**: 15 modules, weekly cycle, SOC 2 compliant

## Key References

| Document | Path |
| -------- | ---- |
| Architecture | [ARCHITECTURE.md](ARCHITECTURE.md) |
| Glossary | [GLOSSARY.md](GLOSSARY.md) |
| Onboarding | [ONBOARDING.md](ONBOARDING.md) |
| Contributing | [CONTRIBUTING.md](CONTRIBUTING.md) |

## Environment

- **Platform**: WSL2 Ubuntu 24.04, 32GB RAM, RTX 2060
- **AI CLIs**: Claude, Codex, Gemini, Aider, Goose, Jules

---

*Maintained by NEXUS, VP Operations*
