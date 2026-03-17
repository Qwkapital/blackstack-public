# BlackStack Platform — Agent Orchestration

This document describes the autonomous agent swarm architecture, coordination model, and dispatch mechanisms used in BlackStack.

---

## Agent Taxonomy

BlackStack operates a **16-agent swarm** organized into three functional tiers:

### Dispatcher Agents (9)

Domain-specific routing agents that classify incoming tasks and delegate to the appropriate specialized agent or MCP server.

| Agent | Domain | Responsibility |
|-------|--------|----------------|
| Compliance Dispatcher | Regulatory | Routes compliance checks, policy validation, standards mapping |
| Code Review Dispatcher | Engineering | Routes code analysis, PR reviews, quality assessments |
| Security Dispatcher | Security | Routes vulnerability scans, secret detection, penetration analysis |
| Ledger Dispatcher | Financial | Routes accounting operations, transaction validation, reconciliation |
| Infrastructure Dispatcher | DevOps | Routes health checks, deployment operations, container management |
| Data Dispatcher | Analytics | Routes data pipeline operations, ETL processes, query optimization |
| Documentation Dispatcher | Knowledge | Routes documentation generation, cross-reference validation |
| Communication Dispatcher | External | Routes notifications, reports, stakeholder updates |
| Research Dispatcher | R&D | Routes experimental tasks, benchmarking, model evaluation |

### Specialized Agents (4)

Focused-capability agents that execute specific task categories:

| Agent | Capability | Tools |
|-------|-----------|-------|
| SWE Agent | Software engineering tasks | Code generation, refactoring, testing |
| PR Agent | Pull request analysis | Diff review, impact assessment, merge readiness |
| Vulnerability Agent | Security assessment | CVE scanning, dependency audit, attack surface analysis |
| Secret Agent | Credential management | Vault operations, key rotation, access control |

### Financial Agents (3)

Quantitative and regulatory agents for financial operations:

| Agent | Focus | Output |
|-------|-------|--------|
| Quant Agent | Quantitative analysis | Risk metrics, portfolio modeling, statistical analysis |
| Risk Agent | Portfolio risk | VaR calculations, stress testing, exposure reports |
| Regulatory Agent | Compliance reporting | Regulatory filings, audit preparation, standards mapping |

## Coordination Model — Redis Stigmergy

Agents coordinate via **stigmergy**, a decentralized pattern inspired by biological systems where agents communicate through environmental state rather than direct messaging.

```
┌──────────┐    write    ┌─────────────┐    read     ┌──────────┐
│  Agent A │────marker──▶│    Redis     │◀──marker────│  Agent B │
│          │             │  (port 6379) │             │          │
│          │◀──read──────│  Stigmergy   │──────read──▶│          │
│          │   markers   │    Layer     │   markers   │          │
└──────────┘             └─────────────┘             └──────────┘
```

### How It Works

1. **Task markers** — When an agent begins work, it writes a marker to Redis indicating task ownership, status, and metadata
2. **Completion signals** — Upon finishing, the agent updates the marker with results and quality scores
3. **Emergent allocation** — Available agents poll for unowned markers matching their capabilities
4. **Conflict resolution** — Atomic Redis operations prevent duplicate task assignment
5. **State decay** — Stale markers expire via TTL, preventing deadlocks from failed agents

### Benefits Over Direct Messaging

- **No single point of failure** — agents operate independently
- **Elastic scaling** — add/remove agents without reconfiguration
- **Fault tolerance** — failed agents' tasks are automatically reassigned
- **Observability** — all coordination state is inspectable in Redis

## Bridge Relay v3.7

The Bridge Relay serves as the authenticated dispatch layer between clients and the agent swarm.

### Authentication Flow

```
Client → Bearer Token → Bridge Relay → Token Validation → Agent Dispatch
                              │
                         Ed25519 Sign
                         TaskPacket
                              │
                         Fan-out to
                         target agents
```

### Task Packet Structure

Every agent operation is wrapped in a signed TaskPacket:

```json
{
  "task_id": "uuid-v4",
  "role": "dispatcher|specialized|financial",
  "agent": "agent-identifier",
  "payload": { },
  "timestamp": "ISO-8601",
  "signature": "Ed25519-base64"
}
```

### Quality Scoring

Agent outputs pass through a quality scoring pipeline before propagation:

1. **Schema validation** — output matches expected structure
2. **Completeness check** — all required fields are present
3. **Confidence threshold** — agent-reported confidence meets minimum
4. **Cross-reference** — entity references validated against registry
5. **Propagation decision** — score determines accept/review/reject

### A2A Protocol

The Agent-to-Agent (A2A) protocol enables inter-agent delegation:

- Agents can delegate subtasks to other agents through the Bridge Relay
- All delegations are subject to 3-level subagent verification
- Delegation chains are tracked for audit purposes
- Maximum delegation depth is configurable (default: 3 levels)

---

*Quantum Wealth Kapital — Autonomous AI Agent Orchestration*
