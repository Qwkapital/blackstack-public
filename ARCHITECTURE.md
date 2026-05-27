# Blackstack Architecture

## System Overview

Blackstack operates as a 5-plane autonomous system with 6 active agents orchestrated through Bridge v3.9.

```
                        ┌─────────────────┐
                        │   FRONT DOOR    │
                        │ Open WebUI      │
                        │ Telegram / CLI  │
                        └────────┬────────┘
                                 │
                        ┌────────▼────────┐
                        │  CONTROL PLANE  │
                        │ NEXUS-Core      │
                        │ Bridge v3.9     │
                        │ DAG Engine      │
                        │ Approval Queue  │
                        └────────┬────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
     ┌────────▼────────┐ ┌──────▼───────┐ ┌───────▼───────┐
     │  WORKER PLANE   │ │  TOOL PLANE  │ │  STATE PLANE  │
     │ Claude (builder)│ │ 45 MCP svrs  │ │ Redis 6379    │
     │ Codex (failover)│ │ n8n 5678     │ │ Qdrant 6333   │
     │ Gemini (research│ │              │ │ Git (main)    │
     │ Ghost (local)   │ │              │ │ Filesystem    │
     │ SENTINEL (fanout│ │              │ │ audit trails  │
     └─────────────────┘ └──────────────┘ └───────────────┘
```

## Planes

### Front Door
User intake is primarily Open WebUI on `model=nexus`, with Telegram as approval/mobile continuity and CLI as admin/worker interface. Requests are converted to structured intents with scope, risk, and constraints.

### Control Plane
NEXUS-Core decomposes goals, classifies tasks (type/scope/risk/autonomy), routes via decision tree, dispatches through Bridge v3.9, monitors execution, and verifies results. Bridge provides routing, dispatch, quality scoring, DAG execution, approval queuing, A2A protocol (v0.2), and Ed25519 audit signing.

### Worker Plane
4 worker agents (Claude, Codex, Gemini, Ghost) + 2 control agents (NEXUS, SENTINEL). Each worker has a defined capability profile, cost tier, and Thompson Sampling win record.

### Tool Plane
45 MCP servers organized by department (core, data, services, platform, presentation). n8n v2.21.5 for workflow automation. 30 Claude Code skills for structured task execution.

### State Plane
Qdrant for episodic memory (BGE-M3 embeddings, 5 collections). Redis for advisory locks and lane signals. Git for versioned persistence. Filesystem for session logs, audit trails, and approval records.

## Agent Topology

| Agent | Provider | Role | Dispatch | Model Tier | Thompson Wins |
|-------|----------|------|----------|------------|---------------|
| NEXUS | Claude (Anthropic) | Orchestrator, control plane | Direct / Bridge | Opus 4.7 (HIGH) | — |
| Claude | Claude (Anthropic) | Builder, reviewer, architect | Bridge /dispatch | Sonnet 4.6 (STANDARD) | — |
| Codex | OpenAI | Failover builder, structured output | Bridge /dispatch | Codex CLI | 10 |
| Gemini | Google | Researcher, validator | Bridge /dispatch | Gemini 2.5 Pro | — |
| Ghost | Ollama (local) | Quality gates, local inference | Bridge /dispatch | qwen3:4b | 31 |
| SENTINEL | Fan-out | Quorum coordinator | Bridge /fan-out | N/A | — |

Thompson Sampling routes HIGH-risk decisions dynamically based on historical win rates (50+ data points target).

## Service Map

| Service | Port | Protocol | Health |
|---------|------|----------|--------|
| Bridge relay | 5679 | HTTP/Bearer | GET /health |
| n8n | 5678 | HTTP | GET /healthz |
| Redis | 6379 | Redis | PING |
| Qdrant | 6333 | HTTP | GET /health |
| Open WebUI | 3000 | HTTP | GET /health |
| Ollama | 11434 | HTTP | GET /api/tags |

## Bridge v3.9 Module Map

```
bridge-relay.js (entry :5679, 2685 lines, 19/20 handlers extracted)
├── lib/routing.js           — Decision tree, provider-aware routing
├── lib/dispatchers.js       — HTTP/CLI/Ollama dispatch adapters
├── lib/dag.js               — Multi-node DAG execution, retry, pause/resume
├── lib/approval-queue.js    — Async approval queue, fs-backed, Telegram notify
├── lib/quality-scorer.js    — Response quality scoring (Haiku 4.5 + Ghost)
├── lib/profiler.js          — Agent performance profiling (quality/speed/cost)
├── lib/stigmergy.js         — Redis advisory locks, lane state
├── lib/standing-orders.js   — SO evaluation engine (14 standing orders)
├── lib/lessons.js           — Experience feedback loop
├── lib/traces.js            — Execution tracing
├── lib/audit.js             — Dispatch audit + Ed25519 signing
├── lib/a2a-protocol.js      — A2A v0.2 (Phase 2 adopted)
├── lib/openai-compat.js     — OpenAI API compatibility + UI model filtering
└── lib/nexus-control-plane.js — model=nexus routing
```

## Data Flow

```
User → Open WebUI (model=nexus) → Classify (type/scope/risk) → Autonomy Level (A0–A4)
  → Route (ROUTING_POLICY decision tree) → Dispatch (Bridge → Agent)
  → Execute → Verify (SENTINEL if HIGH risk) → Document (audit trail + Ed25519)
  → Gate (if A3+: CEO approval) → Close
```

## Autonomy Model

| Level | Scope | Gate | Example |
|-------|-------|------|---------|
| A0 | Full-auto | None | File reads, research |
| A1 | Auto-report | Log | Multi-file edits |
| A2 | Auto-notify | Notify CEO | Git commits, config changes |
| A3 | Propose-approve | CEO approval | PRs, new packages, HIGH risk |
| A4 | CEO-only | CEO executes | Push to main, secrets, billing |

14 standing orders (SO-001–SO-014) pre-approve recurring A3 patterns, reviewed monthly.

## Infrastructure

- **Platform**: WSL2 Ubuntu 24.04, 32GB RAM, RTX 2060 6GB VRAM
- **Docker**: n8n, Redis, Qdrant, Open WebUI (profiles: core/dev/observability)
- **Ollama**: local inference — qwen3:4b, qwen2.5-coder:7b, phi4-mini, BGE-M3 embeddings
- **CI/CD**: 3 GitHub Actions workflows (ci, test-mcp-servers, security-review)
- **Hosting**: quantumwealthkapital.com on Namecheap (VPS migration pending DEC-001)

---

*Maintained by NEXUS, VP Operations — Bridge v3.9 · 45 MCPs · 6 agents · 30 skills*
