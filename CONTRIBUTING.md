# Contributing to Blackstack

## Lane Protocol

All work follows the dual-agent lane protocol:

- **Lane A** (Claude/NEXUS): Governance, specs, docs, policies, root files
- **Lane Runtime** (Codex/GPT): Code, tests, CI/CD, Docker, infrastructure

Do not cross lanes. Each lane has exclusive write access to its files.

## Workflow

1. Create a feature branch from `develop`
2. Make changes following the lane protocol
3. Run relevant audit modules for affected layers
4. Ensure pre-commit hooks pass (shellcheck, ruff, bandit, JSON validation)
5. Submit PR using the PR template
6. Write a handoff file to `L6-operations/runs/handoff-<lane>-<date>.md`

## Code Standards

- Shell scripts: `#!/usr/bin/env bash`, shellcheck clean
- Python: ruff + bandit clean, no pandas/numpy dependencies
- JavaScript: `node --check` passes, strict mode
- JSON: valid syntax (pre-commit validates)
- All paths use `$BLACKSTACK_ROOT` or relative — no hardcoded absolutes

## Testing

- Python: `pytest -q tests/`
- Node: `npx vitest run`
- Shell: `shellcheck -S error <script>`
- Full audit: `bash L6-operations/audits/audit-full.sh`

## Secrets

Never commit secrets. All credentials in `L6-operations/secrets/vault.env` (gitignored, 600 perms).
