# Our n8n Skills — Technical Reference

7 original skills we authored. Source: `L5-infrastructure/n8n-skills/skills/`.

## swarm-task-runner

Routes an arbitrary task through Bridge v3.9 with full governance:

- HTTP POST to `http://127.0.0.1:5679/dispatch` with task payload + risk level
- Bridge applies ROUTING_POLICY: classifies → routes to primary agent → quality-scores
- Returns: agent response, quality score, HITL status, token usage
- Used by: research triggers, compliance checks, document generation

## research-sweep

Cron: daily 06:00. Dispatches to Bridge with `type=research` → Gemini agent →
writes findings to `L8-research/outputs/sweeps/`. Keeps INTELLIGENCE_DIGEST current.

## eb2-niw-monitor

Cron: weekly Monday 09:00.
1. Fetch Visa Bulletin (HTTP GET, State Dept endpoint)
2. Parse EB-2 ROW priority date cutoff
3. Compare to filing readiness score (WES/15 + Letters/30 + Attorney/15 + I-140/10 + Plan/10 + Exhibits/20)
4. If cutoff advanced or score changed: send email alert via gmail-personal MCP

## data-pipeline

Triggered via `mcp-data-pipeline` MCP tool. Orchestrates:
1. Extract from source (Gmail MCP, GSheets MCP, or Firecrawl web crawl)
2. Transform: normalization + entity extraction via Bridge agent dispatch
3. Load to Qdrant (BGE-M3 embeddings) or write to finance-db

## notification-router

Routes alerts by severity:
- CRITICAL → Telegram HITL → Bridge `/approve` gate → email confirmation
- MEDIUM → email via gmail-personal MCP only
- LOW → log to incident-tracker MCP (no interrupt)

## audit-automation

Weekly cron (part of 36-entry crontab). Triggers Bridge `/fan-out` across 5 AEGIS domains
simultaneously → aggregates per-domain JSON results → writes scorecard → alerts on FAIL.

## compliance-workflow

Triggered by pre-commit hook changes to L3-governance/ files. Runs OWASP/NIST/ISO scan.
If new FAIL detected: create incident via incident-tracker MCP → route via notification-router.
