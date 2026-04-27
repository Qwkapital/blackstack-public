# n8n Automation Layer

n8n v2.14.2 is external automation infrastructure we self-host via Docker Compose. This directory
documents the **7 original skills and automation patterns we authored** on top of n8n — our
workflows, our Bridge integration architecture, our automation logic.

## What We Built

### 7 Original Skills

Source: `L5-infrastructure/n8n-skills/skills/` — our intellectual property built on top of n8n.

| Skill | Type | What it does |
|-------|------|--------------|
| swarm-task-runner | Workflow | Routes tasks through Bridge v3.9 `/dispatch` with risk classification |
| research-sweep | Cron | Daily 06:00 sweep → writes findings to L8-research/outputs/ |
| eb2-niw-monitor | Cron | Weekly Visa Bulletin check → filing readiness score → email alert |
| data-pipeline | Workflow | ETL: extract (Firecrawl/GSheets) → transform → load to Qdrant |
| notification-router | Workflow | Multi-channel alerts: CRITICAL→Telegram HITL, MEDIUM→email, LOW→log |
| audit-automation | Cron | Triggers Bridge `/fan-out` for parallel AEGIS domain audit |
| compliance-workflow | Workflow | OWASP/NIST scan → incident-tracker MCP → escalation routing |

### Bridge Integration Architecture

```
n8n workflow node
  → HTTP POST http://127.0.0.1:5679/dispatch  (Bridge v3.9)
  → Bridge routes to Claude / Codex / Gemini / Ghost
  → Result returned to n8n → next workflow node
```

`BRIDGE_TOKEN_N8N` scoped credential in vault.env.
n8n runs at localhost:5678. Bridge at localhost:5679.

## Skills Reference

See [SKILLS.md](SKILLS.md) for per-skill technical specification.

## n8n-mcp Integration

See [MCP_N8N.md](MCP_N8N.md) for how we wired the `n8n-mcp` MCP server into Bridge v3.9.

## Infrastructure

```
L5-infrastructure/runtime/compose/docker-compose.yml  — n8n container config
L5-infrastructure/runtime/scripts/                    — backup, restart, logs
L5-infrastructure/lab/                                — custom nodes (npm run build)
L5-infrastructure/n8n-skills/                         — our 7 authored skills
```

---
*n8n is external software. Our authorship = the 7 skills above + Bridge integration architecture.*
