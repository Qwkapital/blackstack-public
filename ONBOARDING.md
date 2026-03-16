# Agent Onboarding Guide

How to start working in Blackstack as a new or returning agent.

## Step 1: Read These First

1. [README.md](README.md) — system overview, layer structure, quick start
2. [ARCHITECTURE.md](ARCHITECTURE.md) — 5 planes, agent topology, service map
3. [GLOSSARY.md](GLOSSARY.md) — 50+ acronyms and terms

## Step 2: Know Your Lane

| Lane | Owner | Scope | Key Paths |
| ---- | ----- | ----- | --------- |
| **Lane A** (Governance) | Claude (NEXUS) | Specs, policies, contracts, docs | `L3-governance/**`, root `*.md` |
| **Lane Runtime** | Codex/GPT (FORGE) | Code, tests, CI, scripts | `lib/**`, `.github/**`, `tests/**` |

Your lane determines which files you can write. Cross-agent contracts define exact boundaries.

## Step 3: Verify Environment

```bash
# Check services
bash L6-operations/scripts/health.sh

# Check versions
bash L6-operations/scripts/versions.sh

# Check active lanes
bash L6-operations/scripts/lane-worktree.sh list
```

## Step 4: Pick Up a Handoff

1. Check for handoff files: `ls L6-operations/runs/handoff-*.md`
2. Read the most recent handoff for your lane
3. Verify all listed deliverables exist
4. Check for blocking issues
5. If all clear, begin your assigned work

## Step 5: Autonomy Rules

| Level | What You Can Do | Gate |
| ----- | -------------- | ---- |
| A0 | Read files, research, route LOW risk | None |
| A1 | Multi-file edits, MEDIUM routing | Log in session.log |
| A2 | Git commits, config changes | Notify CEO in handoff |
| A3 | PRs, new packages, HIGH risk | CEO approval required |
| A4 | Push to main, secrets, billing | CEO executes |

Check standing orders for pre-approved A3 delegations.

## Step 6: When You're Done

1. Write a handoff file with all deliverables and exact file paths
2. Document any blockers or issues
3. Do NOT leave orphaned lanes — close if you opened one

## Rules to Remember

- **Line limits**: max 150 lines per Write, 100 per Edit, 300 cumulative per phase
- **No sudo**, no hardcoded paths, no secrets in code
- **All output in English** regardless of input language
- **Verify before trusting**: `ls -la` confirms existence, `wc -l` confirms content
- **PRSA protocol** for multi-approach decisions
- **Max 4 round-trips** per task, then escalate

---

*Maintained by NEXUS, VP Operations*
