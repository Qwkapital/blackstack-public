# BlackStack — AI Governance Framework

BlackStack enforces institutional-grade governance across all autonomous AI operations. The framework
maps to established standards and implements continuous enforcement through automated guards, runtime
hooks, audit cycles, and a 4-tier risk classification system.

---

## Standards Alignment

| Standard | Coverage | BlackStack Implementation |
|----------|----------|--------------------------|
| NIST AI RMF 1.0 | GOVERN / MAP / MEASURE / MANAGE | Risk tiers, routing policy, Thompson Sampling metrics, hook guards |
| ISO/IEC 42001:2023 | AI Management System (7 clauses) | L3-governance/ policies, SWARM_DIRECTIVE, audit lifecycle |
| NIST CSF 2.0 | 5 functions | Structurally aligned via COBIT 2019 mapping |
| COBIT 2019 | IT Governance | Decision rights, accountability chains, DEC-xxx format |
| SOC 2 Type I | Security + Availability | Scoped tokens, Ed25519 audit trail, change management |
| OWASP Agentic Top 10 | 10 threats | 7/10 mitigated — see SECURITY.md |
| Google A2A v0.2 | Agent interoperability | Phase 2 adopted (bridge endpoints aligned); Phase 3 pending |

---

## 26-Module Audit Framework

Continuous audit suite producing scored OK/WARN/FAIL reports. Orchestrated by AEGIS across 5 domains.

### AEGIS Domains (5)

| Domain | Script | Modules Orchestrated |
|--------|--------|---------------------|
| Integrity | aegis-integrity.sh | entity-xref, entity-xref-v2, agent-specs, config-coherence, conciliate |
| Compliance | aegis-compliance.sh | coherencia, big4, mcp-compliance |
| Observability | aegis-observability.sh | operational, mcp-health, currency |
| Evidence | aegis-evidence.sh | score, delta, readiness-bundle, audit-trail |
| Dispatch | aegis-dispatch.sh | documental, modelos, cross-refs, enlaces, inventario, tax, shell-quality, quality-integration, remediate |

### Scoring Formula

```
Score = (OK / (OK + WARN + FAIL)) × 100
```

---

## Policy Framework

103 policies in `L3-governance/policies/`. Core governance documents:

| Policy | Purpose |
|--------|---------|
| SWARM_DIRECTIVE | Operational mandates for all agents and the control plane |
| ROUTING_POLICY | Deterministic task routing decision tree with risk gates |
| AUTONOMY_FRAMEWORK | What agents can self-authorize vs. must escalate |
| ANTI_HALLUCINATION_POLICY | Data provenance, canonical source rules, 197-binding guard |
| NEURAL_PROTOCOL | Anti-loop (8 impulse budget), anti-ambiguity, anti-malpractice |
| RESEARCH_PERSISTENCE_POLICY | Write-First rule — all research must persist before returning |
| DATA_CONCILIATION_POLICY | Cross-layer canonical source drift detection (weekly cron) |
| AUTO_ORCHESTRATION | Conditions under which NEXUS may self-trigger agent chains |
| ACCESS_CONTROL_MATRIX | Per-agent, per-endpoint authorization matrix |
| COMPLIANCE_MATRIX | Standards → control → audit module mapping |

---

## Governance Enforcement Chain

```
Pre-commit (10 guards) → staged file validation
  ↓
PreToolUse hook (G1-G10) → live operation gating
  ↓
Bridge dispatch → ROUTING_POLICY risk classification
  ↓
Quality scorer → per-task output evaluation
  ↓
PostToolUse hooks (4) → post-execution validation
  ↓
Weekly audit cron (36 entries) → drift detection + remediation
```

---

## Risk Tier Model

| Tier | Threshold | Routing |
|------|-----------|---------|
| STANDARD | Default | Primary worker via ROUTING_POLICY |
| HIGH | Sensitive data, L1/L2 writes, external comms | SENTINEL fan-out → 3 workers → council → quality gate |
| CRITICAL | Infrastructure changes, push to git | SENTINEL + human approval (Telegram HITL) required |
