# Research

## Published Work

### AI Governance for FDIC-Insured Institutions: A Practitioner's Framework

| Field | Detail |
|-------|--------|
| Published | May 2026 |
| Network | SSRN Financial Economics Network |
| Abstract ID | 6791198 |
| URL | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6791198 |

Existing U.S. federal AI risk frameworks (SR 11-7, OCC Bulletin 2026-13, SR 26-2) were designed for deterministic model risk management. They predate autonomous AI and multi-agent orchestration, leaving all 4,336 FDIC-insured institutions without operational guidance for deploying production agentic systems. This paper documents the governance gap and presents a practitioner methodology validated in production.

**Coverage:**

1. Governance gap analysis -- where SR 11-7 and OCC 2026-13 fail to address multi-agent AI systems
2. Multi-agent orchestration architecture -- audit trail design for agentic workflows
3. 14-module audit framework -- practical implementation of governance controls
4. Production deployment methodology -- validated approach for FDIC-regulated environments
5. Compliance posture assessment protocol -- operational readiness scoring

**Regulatory alignment:**

| Framework | Issued | Gap addressed |
|-----------|--------|---------------|
| SR 11-7 | April 2011 | Model risk management -- predates autonomous AI by 15 years; no agentic workflow coverage |
| OCC Bulletin 2026-13 | 2026 | Establishes AI governance requirement for federal banking; implementation guidance absent |
| SR 26-2 | 2026 | AI model risk (Fed/OCC/FDIC joint) -- extends SR 11-7; does not address multi-agent orchestration |
| Treasury Report | December 2024 | Documents human capital gap in AI governance across FDIC-insured institutions |
| FSOC 2025 | 2025 | Flags AI governance as emerging systemic financial risk |
| GAO-25-107197 | May 2025 | Documents regulatory gap for autonomous AI in regulated industries |

**Reference implementation:** BlackStack v3.0 is the production system this paper documents. It comprises 14 ISO 27001/SOC 2/NIST CSF/COBIT-validated audit modules, deployed at a named U.S. financial services client. The v3.9 development state (26 modules) is tracked in this repository. See [CHANGELOG.md](CHANGELOG.md) for version history.

## Forthcoming

Production deployment case study documenting measurable governance outcomes at a named U.S. client. Data collection: active.
