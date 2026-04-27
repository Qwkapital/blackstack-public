# n8n-mcp Integration — Our Architecture

How we wired the `n8n-mcp` server into BlackStack's Bridge v3.9.

## Registration

`n8n-mcp` is registered in `L4-engineering/mcp-manifest.json`:

```json
"n8n-mcp": {
  "type": "npx",
  "package": "n8n-mcp@2.47.7",
  "pinned": true,
  "status": "active"
}
```

Bridge loads `n8n-mcp` alongside the other 44 MCP servers via `.mcp.json`.

## Endpoint Configuration

| Setting | Value |
|---------|-------|
| n8n base URL | `http://localhost:5678` |
| Bridge URL | `http://localhost:5679` |
| Auth | `N8N_API_KEY` from `vault.env` |
| Bridge token | `BRIDGE_TOKEN_N8N` (scoped: `/dispatch` + `/fan-out` only) |

## Tools We Use

| MCP Tool | When we call it |
|----------|----------------|
| `execute_workflow` | Trigger a named n8n workflow (e.g., swarm-task-runner) |
| `get_workflow_status` | Poll execution status for async workflows |
| `list_workflows` | Enumerate workflows (used by audit-automation skill) |
| `trigger_webhook` | Fire webhook-triggered workflow from Bridge context |

## Integration Pattern

When Bridge dispatches a task of `type=automation`, the dispatch handler calls
`n8n-mcp.execute_workflow` with the task payload. n8n executes the appropriate workflow,
which may call back to Bridge via HTTP for orchestrated tasks.

Anti-loop guard: max 2 re-entries per task (Bridge tracks call depth in task-records/).

## Scoped Token Model

Each service has a scoped `BRIDGE_TOKEN_<SERVICE>` limiting Bridge endpoint access.
n8n token grants: `/dispatch`, `/fan-out`. Cannot call `/approve` or `/v1/chat/completions`.
