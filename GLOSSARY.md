# Blackstack Glossary

## Autonomy & Governance

| Term | Definition |
| ---- | ---------- |
| **A0-A4** | Autonomy tiers: A0 (full-auto), A1 (auto-report), A2 (auto-notify), A3 (propose-approve), A4 (CEO-only) |
| **S0-S2** | Inter-agent delegation tiers: S0 (no gate), S1 (NEXUS validates), S2 (NEXUS decomposes+monitors) |
| **SO** | Standing Order — pre-approved A3 delegation with conditions and expiry (SO-001 to SO-013) |
| **DEC** | Decision document (DEC-001 to DEC-019) in `L2-corporate/strategic-management/decisions/` |
| **GOV-POL** | Governance Policy identifier prefix (93 policies in L3-governance) |
| **BIG4** | Compliance framework alignment: ISO27001, SOC2, NIST-CSF, COBIT-2019 |
| **HITL** | Human-in-the-loop — CEO approval gate for A3+ actions |
| **CAB** | Change Advisory Board — change management review record |
| **PIR** | Post-Incident Review — incident analysis document |

## Agents & Swarm

| Term | Definition |
| ---- | ---------- |
| **NEXUS** | Abstract orchestrator role (currently Claude Opus 4.6); VP Operations |
| **FORGE** | Primary builder agent (currently Codex/GPT) |
| **ATLAS** | Research and validation agent (currently Gemini) |
| **ORACLE** | Refactorer agent (currently Aider) |
| **BOLT** | Executor agent for OS-level tasks (currently Goose, $0 cost) |
| **GHOST** | Local fallback/triage agent (currently Ollama) |
| **SENTINEL** | Fan-out coordinator for HIGH risk quorum decisions |
| **HEAVY** | Async cloud builder (currently Jules) |
| **PRSA** | Propose-Review-Score-Assign — multi-approach decision protocol |
| **Swarm** | The collective of 4 active agents orchestrated by NEXUS through Bridge |

## Infrastructure

| Term | Definition |
| ---- | ---------- |
| **Bridge** | HTTP relay at :5679 that routes, dispatches, and monitors agent tasks (v3.8, 20 lib modules) |
| **MCP** | Model Context Protocol — standard for AI tool servers (40 servers, 192+ tools) |
| **DAG** | Directed Acyclic Graph — multi-step task execution with dependencies |
| **Stigmergy** | Redis-based indirect coordination: advisory locks, lane status, handoff signals |
| **Lane** | Git worktree isolation for parallel multi-agent work (max 2 active + develop) |
| **Synaptic Packet** | Structured context payload sent to agents with task, history, and constraints |
| **LiteLLM** | OpenAI-compatible proxy at :4000 routing to multiple LLM providers |
| **OpenFang** | Model orchestration service at :50051 (97 models, gRPC) |

## Operations

| Term | Definition |
| ---- | ---------- |
| **L1-L8** | Layer numbering: L1 personal, L2 corporate, L3 governance, L4 engineering, L5 infra, L6 ops, L7 marketing, L8 research |
| **Guard** | Hook enforcement check (G1-G10 in hook-guard.sh): file limits, write limits, tool counting |
| **Audit module** | Automated verification script (15 total: 14 in audit-full.sh + conciliate-canonical.sh) |
| **Handoff** | Structured document transferring work context between agents at session boundaries |
| **Scratchpad** | Task tracking file in `L6-operations/runs/` for multi-step operations |
| **Bootstrap** | Environment provisioning script (`L6-operations/scripts/bootstrap.sh`) |

## Corporate

| Term | Definition |
| ---- | ---------- |
| **QWK** | Quantum Wealth Kapital LLC — operating entity |
| **QKT** | QKapital Technologies LLC — parent entity (owns QWK 100%) |
| **JDCM** | Juan David Cardona Mera — CEO and sole principal |
| **EB-2 NIW** | Employment-Based Second Preference, National Interest Waiver — immigration petition |
| **IBKR** | Interactive Brokers — trading platform (blocked until 10/10 maturity) |

---

*Maintained by NEXUS, VP Operations*
