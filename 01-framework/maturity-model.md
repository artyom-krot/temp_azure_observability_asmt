# Observability Maturity Model

Used to score the client's current state per domain and set improvement targets.
Based on CNCF Observability Whitepaper levels, adapted for Azure + Datadog context.

---

## Maturity Levels

| Level | Name | Description |
|---|---|---|
| **0** | None | No observability in this area |
| **1** | Reactive | Basic tooling present; issues discovered after consumer reports |
| **2** | Aware | Monitoring exists; some alerts; investigation is mostly manual |
| **3** | Proactive | SLO-based alerting; MTTD < 5 min for Tier 1; runbooks exist |
| **4** | Predictive | Anomaly detection; capacity planning; automated remediation; CI/CD-driven |
| **5** | Optimized | Full automation; chaos-tested; business-metric-driven SLOs; cost-optimized |

---

## Scoring Sheet

Complete this after the gap analysis phase. Score each domain 0–5.

| Domain | Current Level | Target Level | Gap | Priority |
|---|---|---|---|---|
| D1 — Instrumentation | | | | |
| D2 — Collection & Pipelines | | | | |
| D3 — Alerting | | | | |
| D4 — Dashboards & Visibility | | | | |
| D5 — AKS Observability | | | | |
| D6 — Multi-Env Consistency | | | | |
| D7 — APM & Tracing | | | | |
| D8 — Security Observability | | | | |
| D9 — Operational Processes | | | | |
| D10 — Governance | | | | |
| D11 — Automation & CI/CD | | | | |
| D12 — Datadog Utilization | | | | |

**Overall maturity score:** (average of domain scores) ___/5

---

## Scoring Guidance

When scoring, use the checklist in `assessment-checklist.md` as evidence.
- All items in a domain checked ✅ → candidate for level 4+
- Mix of ✅ and ⚠️ → level 2–3
- Mostly ❌ → level 0–1
- Significant ❓ (unknown) → treat as ❌ until confirmed; unknowns are a finding in themselves

---

## Target State

Recommended minimum target for a production ISV at this scale: **Level 3 across all domains**.
Level 4 for D3 (Alerting), D6 (Multi-env Consistency), and D11 (Automation) given the
multi-tenant, multi-region deployment model.
