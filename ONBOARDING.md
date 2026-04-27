# Agent Onboarding Guide

How to start working in Blackstack as a new or returning agent.

## Step 1: Read These First

1. [README.md](README.md) — system overview, layer structure, quick start
2. [ARCHITECTURE.md](ARCHITECTURE.md) — 5 planes, 6-agent topology, service map
3. [GLOSSARY.md](GLOSSARY.md) — 50+ acronyms and terms
4. [SECURITY.md](SECURITY.md) — 10 hook guards, 26 audit modules, OWASP coverage
5. [GOVERNANCE.md](GOVERNANCE.md) — 53 controls, policy enforcement, audit cycle

## Step 2: Know the Agent Model

| Agent | Role | Dispatch | Strength |
|-------|------|----------|----------|
| NEXUS | Orchestrator (control) | Direct / model=nexus | Goal decomposition, routing, governance |
| Claude | Builder, reviewer, architect | Bridge /dispatch | Code generation, architecture, review |
| Codex | Failover builder | Bridge /dispatch | Structured output, CLI tasks |
| Gemini | Researcher, validator | Bridge /dispatch | Quantitative research, large context |
| Ghost | Local inference | Bridge /dispatch | Offline, privacy, quality scoring |
| SENTINEL | Fan-out coordinator | Bridge /fan-out | Quorum decisions, HIGH risk |

Routing decisions follow `ROUTING_POLICY.md` (decision tree). Cost rule: cheapest adequate provider wins.

## Step 3: Verify Environment

```bash
# System health
bash L6-operations/scripts/health.sh

# Version audit
bash L6-operations/scripts/versions.sh

# Bridge status
curl http://127.0.0.1:5679/health
```

## Step 4: Understand Autonomy Levels

| Level | Scope | Gate |
|-------|-------|------|
| A0 | Full-auto | None |
| A1 | Auto-report | Log only |
| A2 | Auto-notify | Notify CEO |
| A3 | Propose-approve | CEO approval required |
| A4 | CEO-only | CEO executes directly |

14 standing orders (SO-001–SO-014) pre-approve common A3 patterns. See `L3-governance/policies/STANDING_ORDERS.md`.

## Step 5: Dispatch a Task

All tasks go through Bridge at `http://127.0.0.1:5679`:

```bash
# Dispatch via NEXUS (recommended — governed routing)
curl -s -X POST http://127.0.0.1:5679/dispatch \
  -H "Authorization: Bearer $BRIDGE_TOKEN_NEXUS" \
  -H "Content-Type: application/json" \
  -d '{
    "task": "your task description",
    "type": "build",
    "risk": "LOW",
    "agent": "claude"
  }'
```

Task types: `build`, `research`, `review`, `refactor`, `execute`, `validate`  
Risk levels: `LOW` (A0–A1), `MEDIUM` (A2), `HIGH` (A3–A4)

## Step 6: Skills

30 Claude Code skills are available in `~/.claude/skills/`. Invoke via `/skill-name` in any Claude Code session. Skills cover: orchestration, research, audit, docgen, infrastructure, and finance domains.

## Key Paths

| What | Where |
|------|-------|
| Bridge relay | `L5-infrastructure/runtime/scripts/bridge-relay.js` |
| Routing policy | `L3-governance/policies/ROUTING_POLICY.md` |
| Standing orders | `L3-governance/policies/STANDING_ORDERS.md` |
| Agent specs | `L3-governance/specs/agents/*.agent.json` |
| MCP catalog | `L4-engineering/mcp-manifest.json` |
| Health scripts | `L6-operations/scripts/health.sh` |
| Session log | `L6-operations/runs/session.log` |

---

*Maintained by NEXUS, VP Operations*
