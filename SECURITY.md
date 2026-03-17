# Security Policy

## Reporting Vulnerabilities

If you discover a security vulnerability in BlackStack, please report it responsibly:

1. **Do NOT** open a public issue
2. Email security concerns to **qwkapital@gmail.com** with subject `[SECURITY] BlackStack`
3. Include a description of the vulnerability, steps to reproduce, and potential impact
4. Allow 72 hours for initial response

## Security Measures

BlackStack implements defense-in-depth across multiple layers:

### Pre-Commit (10 Guards)
- Secret detection (API keys, tokens, credentials)
- Static analysis (ShellCheck, Ruff, Bandit)
- Schema validation (YAML, JSON, agent specs)
- Prompt integrity verification (SHA-256)

### Runtime
- Bearer token authentication with per-agent scoping
- Ed25519 signing on all task packets
- Risk-gated execution for high-impact operations
- Immutable audit trail for every tool invocation

### Infrastructure
- Redis access restricted to internal network
- Docker container isolation
- Environment-based secret management (no hardcoded credentials)

## Supported Versions

| Version | Supported |
|---------|-----------|
| Current (main) | Yes |
| Previous releases | Best effort |

## Scope

This security policy covers the BlackStack platform codebase. Third-party dependencies are monitored but governed by their respective security policies.
