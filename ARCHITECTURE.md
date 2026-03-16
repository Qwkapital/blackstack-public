# Blackstack Architecture

## System Overview

Blackstack operates as a 5-plane autonomous system with 16 agents orchestrated through Bridge v3.9.

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
     │ FORGE (Codex)   │ │ 40 MCP svrs  │ │ Redis 6379    │
     │ ATLAS (Gemini)  │ │ n8n 5678     │ │ Git (develop) │
     │ ORACLE (Aider)  │ │ OpenFang     │ │ Filesystem    │
     │ BOLT (Goose)    │ │ LiteLLM 4000 │ │ session.log   │
     │ GHOST (Ollama)  │ │              │ │ audit trails  │
     │ SENTINEL (fan)  │ │              │ │               │
     └─────────────────┘ └──────────────┘ └───────────────┘
```

## Planes

### Front Door
User intake via Open WebUI on `model=nexus`, with Telegram as approval/mobile continuity and CLI as admin/worker interface. Requests are converted to structured intents with scope, risk, and constraints.

### Control Plane
NEXUS-Core decomposes goals, classifies tasks (type/scope/risk/autonomy), routes via decision tree, dispatches through Bridge, monitors execution, and verifies results. Bridge v3.9 provides runtime modules for routing, dispatch, quality scoring, DAG execution, approval queuing, and NEXUS Console visibility.

### Worker Plane
9 dispatcher agents + 4 direct + 1 special + 1 fan-out + 1 local. Each agent has a codename, provider, and capability profile defined in agent spec files.

### Tool Plane
40 MCP servers organized by department (core, data, services, platform, presentation, agents). n8n workflows for automation. OpenFang for model orchestration.

### State Plane
Redis for stigmergy (lane status, advisory locks, handoff signals). Git for versioned persistence. Filesystem for session logs, audit trails, and approval records.

## Agent Topology

| Codename | Provider | Role | Dispatch | Cost |
| -------- | -------- | ---- | -------- | ---- |
| NEXUS | Claude (Anthropic) | Architect, Orchestrator | Direct CLI | Premium |
| FORGE | Codex (OpenAI) | Primary Builder | Bridge /dispatch | Premium |
| ATLAS | Gemini (Google) | Researcher, Validator | Bridge /dispatch | Budget |
| ORACLE | Aider | Refactorer | Bridge /dispatch | Variable |
| BOLT | Goose (Block) | Executor, OS tasks | Bridge /dispatch | $0 |
| GHOST | Ollama (local) | Triage, Quality scoring | Bridge /dispatch | $0 |
| SENTINEL | Fan-out | Quorum Coordinator | Bridge /fan-out | N/A |
| HEAVY | Jules (Google) | Async cloud Builder | Direct (async) | Premium |

## Bridge v3.9 Module Map

```
bridge-relay.js (entry point :5679)
├── lib/routing.js         — Decision tree, load-aware, provider-aware
├── lib/dispatchers.js     — HTTP/CLI/Ollama dispatch adapters
├── lib/dag.js             — Multi-node DAG execution, retry, pause/resume
├── lib/approval-queue.js  — Async A3 queue, fs-backed, Telegram notify
├── lib/quality-scorer.js  — Response quality scoring
├── lib/profiler.js        — Agent performance profiling
├── lib/stigmergy.js       — Redis advisory locks, lane state
├── lib/standing-orders.js — SO evaluation engine (13 orders)
├── lib/self-reflection.js — Agent self-assessment
├── lib/lessons.js         — Experience feedback loop
├── lib/traces.js          — Execution tracing
├── lib/cache.js           — Redis exact-match caching
├── lib/audit.js           — Dispatch audit logging
├── lib/a2a-protocol.js    — Agent-to-agent protocol
├── lib/synaptic-packet.js — Context packet builder
├── lib/plan-document.js   — Plan artifact management
├── lib/openai-compat.js   — OpenAI API compatibility layer
├── lib/system-integration.js — MCP tool loading
└── lib/nexus-control-plane.js — model=nexus routing
```

## Data Flow

```
User Request → Open WebUI (model=nexus) → Classify (type/scope/risk) → Autonomy Level (A0-A4)
  → Route (decision tree) → Dispatch (Bridge → Agent) → Execute
  → Verify (independent reviewer) → Document (audit trail) → Gate (if A3+: CEO approval) → Close
```

## Autonomy Model

| Level | Scope | Gate | Example |
| ----- | ----- | ---- | ------- |
| A0 | Full-auto | None | File reads, research, routing LOW |
| A1 | Auto-report | Log | Multi-file edits, MEDIUM routing |
| A2 | Auto-notify | Notify CEO | Git commits, config changes |
| A3 | Propose-approve | CEO approval | PRs, new packages, HIGH risk |
| A4 | CEO-only | CEO executes | Push to main, secrets, billing |
| S0-S2 | Agent-to-agent | Bridge logs/validates | Inter-agent delegation |

13 standing orders (SO-001 to SO-013) pre-approve recurring A3 patterns, reviewed monthly.

## Infrastructure

- **Platform**: WSL2 Ubuntu 24.04, 32GB RAM, RTX 2060 6GB VRAM
- **Docker**: n8n, Redis, Open WebUI, LiteLLM, Grafana, Loki (profiles: core/dev/observability)
- **Ollama**: 8 local models (~14.5GB)
- **CI/CD**: 3 GitHub Actions workflows (ci.yml, test-mcp-servers.yml, security-review.yml)

---

*Maintained by NEXUS, VP Operations*
