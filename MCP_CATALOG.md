# BlackStack MCP Server Catalog

45 MCP servers across 5 departments — 192+ tools. All Python servers use FastMCP; all JS servers
use `@modelcontextprotocol/sdk`. Transport: stdio (no HTTP transport unless stated).

## core (3 active)

| Server | Tools | Purpose |
|--------|-------|---------|
| nexus-swarm | 15 | Swarm orchestration — task routing, agent dispatch, fan-out |
| nexus-ops | 12 | Operations — health checks, state management, Qdrant queries |
| mcp-factory | 8 | MCP server scaffolding and lifecycle management |

## data (5 active)

| Server | Tools | Purpose |
|--------|-------|---------|
| mcp-analytics | 8 | DuckDB analytics over L2-L8 data |
| mcp-data-pipeline | 6 | ETL orchestration, data ingestion workflows |
| finance-db | 10 | Read-only access to L2-corporate/finance/ ledgers |
| mcp-storage-unified | 5 | Unified filesystem + blob storage operations |
| knowledge-graph | 9 | Knowledge graph queries over domain entities |

## services (12 active)

| Server | Tools | Purpose |
|--------|-------|---------|
| compliance-engine | 12 | OWASP/NIST/ISO compliance checks, policy enforcement |
| health-monitor | 8 | System health aggregation, SLA tracking |
| docker-manager | 7 | Container lifecycle, volume, network management |
| incident-tracker | 6 | Incident creation, escalation, resolution workflow |
| crontab-manager | 5 | Cron job management for 36-entry audit crontab |
| secrets-vault | 6 | Vault abstraction, secret rotation |
| gmail-personal | 8 | Personal Gmail: search, read, draft, send, labels |
| mcp-auth-gateway | 8 | Token scoping, auth flow, API key management |
| mcp-comms-engine | 7 | Multi-channel notifications (Telegram, email) |
| mcp-finance-engine | 9 | Financial calculations, FX, portfolio operations |
| mcp-observability | 10 | Metrics, traces, logs — OTel-compatible |
| telegram-bot | 5 | HITL approval channel for HIGH risk decisions |

## platform (4 active)

| Server | Tools | Purpose |
|--------|-------|---------|
| docgen | 18 | Document generation from templates (flagship server) |
| mcp-web-research | 8 | Web research coordination, content extraction |
| mcp-pdf-composer | 6 | PDF assembly, annotation, form handling |
| test-harness | 7 | Automated test execution, quality gate runner |

## presentation (5 active)

| Server | Tools | Purpose |
|--------|-------|---------|
| chart-generator | 8 | Data visualization from financial and research data |
| mcp-brand-toolkit | 5 | Brand asset management, color/typography |
| mcp-presentation-builder | 9 | Slide deck generation from research outputs |
| mcp-ui-templates | 6 | UI component library, template rendering |
| mcp-pdf-composer | 6 | PDF assembly (shared with platform) |

## NPX/Node Servers (12 active)

| Server | Package | Purpose |
|--------|---------|---------|
| memory | @modelcontextprotocol/server-memory | Shared working memory across sessions |
| redis | @modelcontextprotocol/server-redis | Redis operations, key-value state |
| tavily | tavily-mcp | Web search with structured results |
| n8n-mcp | n8n-mcp@2.56.0 | n8n workflow execution via MCP |
| context7 | @upstash/context7-mcp | Library documentation retrieval |
| sequential-thinking | @modelcontextprotocol/server-sequential-thinking | Multi-step reasoning |
| obsidian | obsidian-mcp | Obsidian vault read/write |
| playwright | @playwright/mcp | Browser automation, CDP-based interaction |
| firecrawl | firecrawl-mcp | Web-to-markdown, autonomous agent crawl |
| gsheets | (custom) | Google Sheets read/write |

---
*39 servers documented · 45 total including research & specialist stack · Last updated: 2026-05-27*
