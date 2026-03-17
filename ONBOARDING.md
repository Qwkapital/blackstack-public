# Agent Onboarding Guide

How to start working in Blackstack as a new or returning agent.

## Step 1: Read These First

1. [README.md](README.md) — system overview, layer structure, quick start
2. [ARCHITECTURE.md](ARCHITECTURE.md) — 5 planes, agent topology, service map
3. [GLOSSARY.md](GLOSSARY.md) — 50+ acronyms and terms
4. [CLAUDE.md](CLAUDE.md) — workspace rules, paths, limits, swarm architecture
5. [L3-governance/contracts/CROSS_AGENT_CONTRACT.md](L3-governance/contracts/CROSS_AGENT_CONTRACT.md) — lane protocol, handoffs, conflict resolution

## Step 2: Know Your Lane

| Lane | Owner | Scope | Key Paths |
| ---- | ----- | ----- | --------- |
| **Lane A** (Governance) | Claude (NEXUS) | Specs, policies, contracts, docs | `L3-governance/**`, root `*.md` |
| **Lane Runtime** | Codex/GPT (FORGE) | Code, tests, CI, scripts | `lib/**`, `.github/**`, `tests/**` |

Your lane determines which files you can write. See `CROSS_AGENT_CONTRACT.md` Section 1 for exact boundaries.

## Step 3: Verify Environment

```bash
# Check services
bash L6-operations/scripts/health.sh

# Check versions
bash L6-operations/scripts/versions.sh

# Check active lanes
bash L6-operations/scripts/lane-worktree.sh list

# Check pending approvals
ls L6-operations/runs/pending-approvals/ 2>/dev/null
```

## Step 4: Pick Up a Handoff

1. Check for handoff files: `ls L6-operations/runs/handoff-*.md`
2. Read the most recent handoff for your lane
3. Verify all listed deliverables exist: `ls -la <each file>`
4. Check for blocking issues: `ls L6-operations/runs/issue-*.md`
5. If all clear, begin your assigned work

## Step 5: Autonomy Rules

| Level | What You Can Do | Gate |
| ----- | -------------- | ---- |
| A0 | Read files, research, route LOW risk | None |
| A1 | Multi-file edits, MEDIUM routing | Log in session.log |
| A2 | Git commits, config changes | Notify CEO in handoff |
| A3 | PRs, new packages, HIGH risk | CEO approval required |
| A4 | Push to main, secrets, billing | CEO executes |

Check standing orders (`L5-infrastructure/runtime/state/standing-orders.json`) for pre-approved A3 delegations.

## Step 6: When You're Done

1. Write handoff: `L6-operations/runs/handoff-<lane>-<YYYY-MM-DD>-s<N>.md`
2. Use template: `L6-operations/templates/handoff-template.md`
3. List all deliverables with exact file paths
4. Document any blockers or issues
5. Do NOT leave orphaned lanes — close if you opened one

## Rules to Remember

- **Line limits**: max 150 lines per Write, 100 per Edit, 300 cumulative per phase
- **No sudo**, no hardcoded paths, no secrets in code
- **All output in English** regardless of input language
- **Verify before trusting**: `ls -la` confirms existence, `wc -l` confirms content
- **PRSA protocol** for multi-approach decisions
- **Max 4 round-trips** per task, then escalate

## Getting Help

- Operations manual: [L6-operations/OPERATIONS_MANUAL.md](L6-operations/OPERATIONS_MANUAL.md)
- Infrastructure: [L5-infrastructure/INFRASTRUCTURE_ARCHITECTURE.md](L5-infrastructure/INFRASTRUCTURE_ARCHITECTURE.md)
- GPT-5.4 specific: [L3-governance/specs/sops/gpt54-operations.sop.md](L3-governance/specs/sops/gpt54-operations.sop.md)
- Escalation: Agent → PRSA quorum → CEO

---

*— NEXUS, VP Operations*
