# BlackStack — Claude Code Skills

30 skills deployed in the swarm. Skills live in `~/.claude/skills/` and are invoked via
`/<skill-name>` in Claude Code sessions. Each is a SKILL.md spec with instructions and examples.

## Core Orchestration (5)

| Skill | Purpose |
|-------|---------|
| dispatch | Route a task through Bridge v3.9 with risk classification |
| fan-out | Parallel dispatch to 3 workers → SENTINEL aggregation |
| council | Convene deliberation council for HIGH risk decisions |
| approve | Trigger HITL approval gate via Telegram |
| status | Live system status: agents, MCPs, Qdrant, Redis, Bridge |

## Research & Intelligence (5)

| Skill | Purpose |
|-------|---------|
| research | Launch Gemini research agent with Write-First protocol |
| sweep | Trigger automated research sweep on a topic |
| synthesize | Synthesize multiple research outputs into a document |
| digest | Extract key findings from an L8-research output file |
| market-scan | Competitive intelligence sweep on a target domain |

## Audit & Compliance (5)

| Skill | Purpose |
|-------|---------|
| audit | Run 15-module full audit (AEGIS orchestration) |
| audit-delta | Targeted audit on recently changed files |
| audit-score | Current scorecard across all layers |
| compliance-check | OWASP/NIST/ISO compliance verification |
| stamp | Apply BIG4 headers to a directory of files |

## Document Generation (5)

| Skill | Purpose |
|-------|---------|
| docgen | Generate document from template via docgen MCP |
| report | Structured VP-level report on a topic |
| memo | CEO-level decision memo (DEC-xxx format) |
| brief | One-page executive brief from research outputs |
| pdf | Compose and render PDF from markdown source |

## Infrastructure & Operations (5)

| Skill | Purpose |
|-------|---------|
| health | health.sh + heartbeat check across all services |
| deploy | Deploy/restart a service or MCP server |
| backup | Home backup + Docker volume snapshot |
| version-check | versions.sh + compare against version-manifest.json |
| rollback | DEC-009 rollback gate for a target service |

## Finance & Regulatory (5)

| Skill | Purpose |
|-------|---------|
| ledger | Query L2-corporate/finance/ ledgers via finance-db MCP |
| regulatory-status | Filing readiness score + regulatory compliance benchmark |
| entity-check | Validate entity data against entity-registry.json |
| tax | audit-tax.sh compliance verification |
| conciliate | conciliate-canonical.sh cross-layer data drift check |

---
*30 skills total · Updated: 2026-05-27*
