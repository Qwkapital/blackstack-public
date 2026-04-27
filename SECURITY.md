# Security Policy

## Reporting Vulnerabilities

If you discover a security vulnerability in BlackStack, please report responsibly:

1. **Do NOT** open a public issue
2. Email security concerns to **qwkapital@gmail.com** with subject `[SECURITY] BlackStack`
3. Include description, steps to reproduce, and potential impact
4. Allow 72 hours for initial response

## Security Architecture

BlackStack implements defense-in-depth across 53 mechanical controls.

### Pre-Commit Hook (10 Guards)

Fires on every `git commit` — blocks on failure:

| Guard | What it catches |
|-------|----------------|
| Secret detection | API keys, tokens, credentials, .pem, .key files |
| Pattern scanning | Hardcoded secret patterns in staged content |
| ShellCheck | Shell script errors (staged .sh files) |
| Ruff E9/F6/F7/F8 | Python syntax and import errors |
| yamllint | YAML structure validation |
| Bandit | Python security issues (-ll severity) |
| JSON validation | JSON syntax validation on all staged .json files |
| Large file gate | Blocks files > 500KB |
| Agent spec validation | Structural integrity of .agent.json files |
| Prompt integrity | SHA-256 verification of all rule files |

### PreToolUse Hook Guards (G1–G10)

Fires before every Read/Write/Edit/Task operation:

| Guard | Enforcement |
|-------|------------|
| G1 | File size limits (200 lines) + banned file protection |
| G2 | Write limit (150 lines per operation) |
| G3 | Edit limit (100 lines) + cumulative 300-line phase cap |
| G4 | Tool counter (warn 15, critical 20, block 25) |
| G5 | Max 2 parallel agents |
| G6 | Audit scorecard currency (> 7 days = warn) |
| G7 | Cumulative read tracking (block at 1,500 lines) |
| G8 | Agent spec write protection |
| G9 | Identity integrity — blocks hallucinated PII writes |
| G10 | WebSearch bloat prevention |

### PostToolUse Hooks (4 Guards)

| Hook | Purpose |
|------|---------|
| hook-log-action.sh | Logs all mutations to tamper-evident session.log |
| verify-subagent-output.sh | Detects silent subagent write failures |
| guard10-cv-postcheck.sh | Validates 25 CV fields after L1 edits |
| guard11-entity-xref-postcheck.sh | Validates 197 entity identifier bindings after L1/L2 edits |

### Ed25519 Audit Signing

Every session log entry is signed with an Ed25519 key pair (`audit-signing.key` / `.pub`).
Tamper-evident chain — any modification to historical log entries breaks verification.

### Runtime

- Bearer token auth with per-agent scoping (`BRIDGE_TOKEN_<AGENT>`)
- 1MB body limit on all Bridge endpoints
- SSRF partial mitigation on HTTP-capable MCP tools (allowlist pending — see OWASP AA7)
- Docker container isolation, Redis restricted to internal network
- No secrets in tracked files — all secrets in `vault.env` (permissions 600)

## OWASP Agentic Top 10 (2025) Status

| # | Threat | Status |
|---|--------|--------|
| AA1 | Prompt / Eval Injection | OPEN — regex scanner only; ML detection planned |
| AA2 | Insecure Output Handling | PARTIAL — PostToolUse CV + entity hooks |
| AA3 | Excessive Agency | MITIGATED — AUTONOMY_FRAMEWORK blocks self-dispatch |
| AA4 | Overreliance on LLM | MITIGATED — SENTINEL fan-out + council vote for HIGH risk |
| AA5 | Excessive Permissions | MITIGATED — G1 hook + bridge-token scoping |
| AA6 | Sensitive Info Disclosure | MITIGATED — vault.env 600 perms; no secrets in tracked files |
| AA7 | Insecure Plugin Design (MCP) | OPEN — SSRF allowlist not yet deployed |
| AA8 | Insufficient Logging | MITIGATED — session.log + Ed25519 audit signing |
| AA9 | Insecure Code Execution | OPEN — no sandboxed execution deployed |
| AA10 | AI Supply Chain Risk | PARTIAL — LiteLLM abstraction; 3-provider routing |

## Supported Versions

| Version | Supported |
|---------|-----------|
| Current (main) | Yes |
| Previous releases | Best effort |
