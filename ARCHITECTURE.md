<!-- BIG4-HEADER
doc_id: GOV-ARCH-001
version: 2.0
last_audit: 2026-03-07
status: ACTIVE
owner: NEXUS
classification: INTERNAL
-->

# BlackStack Architecture

**Document ID**: GOV-ARCH-001
**Version**: 2.0
**Effective Date**: 2026-02-17
**Owner**: CEO (Juan David Cardona Mera)
**Classification**: L3 INTERNAL
**Review Cycle**: Quarterly

---

## Purpose

BlackStack is an auditable-by-design system. The filesystem is the single source of truth.
All layers have bounded responsibilities; no layer bleeds into another.

## 8-Layer Architecture

| Layer | Directory | Role |
|-------|-----------|------|
| L1-principal | `L1-principal/` | JDCM personal — health, finance, education, relationships, immigration, goals, profile, knowledge/ |
| L2-corporate | `L2-corporate/` | QWK LLC / QKT — entity, strategy, finance, resolutions, compliance, team, projects, custody, decisions |
| L3-governance | `L3-governance/` | BlackStack tech-only — policies, specs, catalog, runbooks (zero JDCM dependency) |
| L4-engineering | `L4-engineering/` | 39 MCP servers by dept (core, data, services, platform, presentation) |
| L5-infrastructure | `L5-infrastructure/` | n8n, Dify, Docker runtime, Bridge Relay v3.8 |
| L6-operations | `L6-operations/` | Scripts, hooks, audits, monitoring, secrets vault |
| L7-marketing | `L7-marketing/` | Brand, landing pages, design system, web assets |
| L8-research | `L8-research/` | R&D sandboxes, experiments, research/ |

## Cascade & Segregation

```
JDCM (L1) → QKT Trust (L2) → QWK LLC (L2) → BlackStack (L3–L8) → NEXUS (transversal)
```

- **L1**: personal data only — never referenced by automated systems
- **L2**: money, legal, strategy — no automation writes here without CEO approval
- **L3**: tech governance, autonomous — zero dependency on L1 or L2 personal data
- **L4–L8**: execution layers — governed by L3 policies

## Invariants

- Evidence never leaves `L6-operations/`
- Normative documents live only in `L3-governance/`
- `L5-infrastructure/` owns its CLAUDE.md for domain-specific instructions
- Git repos live in product context or `L8-research/` sandboxes
- No hardcoded paths — all scripts use `$BLACKSTACK_ROOT`
- Secrets live in `L6-operations/secrets/vault.env` (permissions 600)

## Cross-Layer Communication

```text
L3-governance/     --[policies/schemas]-------> L4, L5, L6 (enforcement)
L6-operations/     --[audit reports]----------> L3-governance/ (evidence)
L5-infrastructure/ --[Bridge localhost:5679]---> L6-operations/ (heartbeat, health)
L5-infrastructure/ --[Docker port 5678]--------> external (n8n API)
L4-engineering/    --[MCP tools]---------------> L5, L6, L7 (execution)
```

## Change History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| v1.0 | 2026-02-08 | NEXUS | Initial document |
| v1.1 | 2026-02-15 | NEXUS | Added change history (Big 4 compliance) |
| v2.0 | 2026-02-17 | NEXUS | Full rewrite — 8-layer architecture, removed obsolete refs (TOOLS_INVENTORY.tsv, obsidian.contract.md), added L2-corporate and L7-marketing |
