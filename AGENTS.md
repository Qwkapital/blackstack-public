# BlackStack — Agent Architecture

BlackStack operates a **6-agent swarm** organized into two tiers: control and worker.
13 legacy codenames (atlas, bolt, forge, oracle, etc.) are archived — canonical worker
model active since Bridge v3.9 (2026-04-08).

---

## Control Tier

| Agent | Role | Governance |
|-------|------|------------|
| NEXUS | Control plane — routes every task via ROUTING_POLICY decision tree | Risk classification → dispatch → quality scoring → HITL gate |
| SENTINEL | Cross-validation, fan-out, council orchestration for HIGH risk | 3 independent opinions minimum; human required if spread > 0.3 |

**NEXUS flow**: Open WebUI / CLI → NEXUS classifies (risk, type, domain) → Bridge v3.9 routes
to worker → quality-scores output → applies HITL gate if HIGH risk.

---

## Worker Tier

| Agent | Specialization | Model | Thompson Wins |
|-------|---------------|-------|----------------|
| Claude | Primary builder, reviewer, architect | claude-sonnet-4-6 (standard) / claude-opus-4-7 (HIGH) | Primary lane |
| Codex | Failover builder, structured output | OpenAI Codex CLI | 10 (building baseline) |
| Gemini | Researcher, SAGE judge, council tiebreaker | Gemini 2.5 Pro / Flash (free tier) | Researcher lane |
| Ghost | Local inference, privacy gate, quality gate | Ollama qwen3:4b (RTX 2060, 6GB VRAM) | 31 (local baseline) |

**Thompson Sampling routing**: dynamic agent selection based on historical win rates per task type.
Each task outcome updates the multi-armed bandit — optimal agent floats to top over time.

---

## Routing Protocol

```
Task → NEXUS classify(risk, type, domain)
  → STANDARD: route to primary worker by type
  → HIGH:     SENTINEL fan-out → 3 workers → vote/consensus → quality-score → HITL
  → CRITICAL: council + human approval required
```

**Task type → primary agent**:
- build / fix / review → Claude (Sonnet 4.6)
- architecture / plan → Claude (Opus 4.7)
- research / quantitative → Gemini
- failover / structured output → Codex
- local / privacy → Ghost
- HIGH risk → Council (Claude + Codex + Gemini) via SENTINEL

---

## Council Protocol

Activated by SENTINEL for HIGH risk decisions:

- **Code / reasoning tasks**: voting (ACL 2025: +13.1% accuracy)
- **Knowledge / factual tasks**: consensus (ACL 2025: +4.9% accuracy)
- **Spread > 0.3**: escalate to human review
- **Max 1 deliberation round** per task

---

## Anti-Loop Controls

- Max 8 impulse budget per task (NEURAL_PROTOCOL)
- Same agent 3× on same task = HARD STOP
- A dispatches to B, B dispatches back to A = STOP (ping-pong)
- Timeouts: Ghost 60s · Codex 120s · Gemini 180s · Claude 120s

---

## Key Governance Files

| File | Purpose |
|------|---------|
| `ROUTING_POLICY.md` | Full decision tree with risk gates and fallback chains |
| `AUTONOMY_FRAMEWORK.md` | What agents can self-authorize vs. must escalate |
| `SWARM_DIRECTIVE.md` | Operational mandates for all agents |
| `NEURAL_PROTOCOL.md` | Anti-loop, anti-ambiguity, anti-malpractice controls |
| `COORDINATION_MODEL.md` | Agent coordination patterns and closeout behavior |
