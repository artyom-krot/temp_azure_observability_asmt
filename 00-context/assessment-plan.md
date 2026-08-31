# Assessment Plan — 4-Week Engagement

**Team:** 2 specialists
**Timeline:** 4 weeks
**Approach:** Evidence-first, pain-points-first, architecture-vision outcome

---

## Strategic Framing

This assessment goes beyond reviewing tool configurations.
The primary outcome is a **target observability architecture** — a clear picture of what good looks like for this platform, how far the current state is from that target, and a prioritised, realistic path to close the gap.

The architecture must address standardised instrumentation, governance, and operational practices that will scale as the platform continues its cloud-native transition.

---

## Scope

### In Scope — Phase 1 (this engagement)

| Focus area | Domains |
|---|---|
| Application and infrastructure landscape | INSTR, COLL |
| Current observability coverage state | INSTR, COLL, ALERT |
| Alerting and detection gaps (Critical/High) — recurring issues, detection approach | ALERT |
| SLA/SLO requirements — technical observability | INSTR, ALERT, OPS |
| General operations strategy | OPS |
| Existing governance model | GOV |
| General observability gaps and blind spots | All in-scope domains |
| Security and compliance (high-level) | SEC |

### Out of Scope — Phase 2

| Topic | Why deferred |
|---|---|
| Datadog capability utilisation optimisation | Requires stable baseline first |
| Business activity monitoring (BAM) — consumer KPIs, SLA compliance, business metrics | Could be extremly wide based on application landscape |
| Monitoring-as-code / CI/CD automation | Depends on target architecture being defined |
| Unified monitoring architecture  | Design |
| Standardise observability: guardrails, ownership, unified toolset | Governance model |
| Apply at scale / Migration strategy under unified observability | Requires multiple teams efforts |


---

## Team Split

| Specialist | Role | Primary responsibility | Secondary support |
|---|---|---|---|
| **Specialist A** | Lead Engineer | Application infrastructure landscape, Logs collection, technical capabilities | — |
| **Specialist B** | Systems Architect | Governance model, Operational strategy, Observability coverage, Recurring issues, Assessment approach, Reporting | Infrastructure |

**Specialist A** brings hands-on technical capabilities to assess the application and infrastructure landscape, evaluate log collection pipelines and APM agent coverage firsthand, and produce evidence-based technical findings.

**Specialist B** defines the assessment approach, reviews observability coverage and recurring detection issues, evaluates the governance model and operational strategy, and owns the structural layers of the target architecture.

Gap synthesis and target architecture drafting are shared.

---

## Week-by-Week Plan

> **Assessment focus:** Evidence-first across all in-scope domains. Priority order: alerting gaps and detection failures first, then instrumentation and coverage depth, then collection pipelines, SLA/SLO gaps, governance, and operational practices.

---

### Week 1 — Discovery & Baseline

**Goal:** Establish the factual baseline. Understand all application and infrastructure tiers, what signals are being emitted, and what is reaching monitoring systems.

| Day | Activity | Specialist | Output |
|---|---|---|---|
| 1 | Datadog read access setup; pull monitors, dashboards, hosts, log indexes, SLOs via API | A | Raw DD exports |
| 1 | Azure read access setup; enumerate subscriptions, resource inventory | B | Resource list |
| 1–2 | Datadog analysis: log sources per tier, log pipeline depth, retention, APM agent inventory | A | `02-discovery/datadog/` |
| 1–2 | Datadog alert inventory: total monitors, basic quality indicators (% with runbook, % with routing) | B | `02-discovery/datadog/` |
| 2–3 | Azure analysis: diagnostic settings coverage, Log Analytics topology, infrastructure inventory (AKS Container Insights check, VM coverage) | B | `02-discovery/azure-monitor/`, `02-discovery/aks/` |
| 4 | **Workshop #1 — Engineering team** | Both | Raw notes |
| 4 | Process workshop notes → structured discovery | Both | `00-context/workshops/` |
| 5 | Identify landscape gaps; consolidate open questions for Week 2 | Both | Updated open questions |

**Must have by end of Week 1:**
- [ ] Full application tier map confirmed: CDN → Apache → microservices → DB (and any other tiers)
- [ ] Log coverage confirmed per tier: what logs exist, where they flow, what tiers are dark
- [ ] Datadog alert inventory: total monitors, basic quality indicators (% with runbook, % with routing)
- [ ] Log retention confirmed: 30-day online; archive present/absent
- [ ] DB crash incident: confirmed whether detected by monitoring or consumers, approximate MTTD

**Workshop #1 focus (Engineering):**
- What services and infrastructure tiers exist? Walk us through the full stack.
- Where do logs come from for each tier? Any tiers with no visibility?
- Walk us through the DB crash: what happened, how was it detected, how long to detect and resolve?
- What alerts fire most frequently? Are they actionable or mostly noise?
- What application metrics are currently emitted? Custom metrics in Datadog?
- What does a "healthy system" look like to you — how do you know things are working?

---

### Week 2 — Technical & Operational Assessment

**Goal:** Assess detection and response quality. Evaluate technical SLO monitoring, alert coverage and noise levels, and map the operations workflow end-to-end. Begin governance review.

| Day | Activity | Specialist | Output |
|---|---|---|---|
| 1–2 | SLA/SLO definitions (client to provide) → map to existing Datadog SLO monitors; identify gaps | A | `02-discovery/datadog/` |
| 1–2 | Alert quality deep dive: coverage gaps, recurring false positives, routing, runbook linkage | B | `02-discovery/datadog/` |
| 1–2 | Governance review: Datadog RBAC, tag strategy, monitor ownership model | B | `02-discovery/datadog/` |
| 3 | Security and compliance: WAF logs, audit events, NSG flow logs — high-level only | B | `02-discovery/azure-monitor/` |
| 4 | **Workshop #2 — Operations / On-call team** | Both | Raw notes |
| 4 | Process workshop notes | Both | `00-context/workshops/` |
| 5 | Consolidate all discovery; update open questions | Both | Updated discovery files |

**Must have by end of Week 2:**
- [ ] Alert quality baseline: % with runbook link, % with notification routing, top recurring noise alerts named
- [ ] Technical SLO gap: SLA/SLO definitions received and mapped to Datadog SLO monitors (or confirmed absent)
- [ ] Governance gaps documented: RBAC state, tag enforcement, monitor ownership

**Workshop #2 focus (Operations / On-call):**
- Walk us through a recent incident end-to-end: alerted, investigated, resolved. What was the timeline?
- What are the most common alerts you receive? Which ones are genuinely useful vs. noise?
- What alerts do you wish existed that don't?
- Do runbooks exist for Tier 1 alerts? Are they up to date and actually used?
- How are SLA compliance reports generated today — automated via monitoring, or manually?
- What's your biggest blind spot — what would you most like to be alerted on but aren't?

---

### Week 3 — Gap Analysis & Validation

**Goal:** Convert all discovery evidence into structured findings. Identify quick wins. Produce an interim findings review with the client. Start the target architecture draft.

| Day | Activity | Specialist | Output |
|---|---|---|---|
| 1 | Gap analysis: INSTR, COLL | A | `03-analysis/findings/INSTR_findings.md`, `COLL_findings.md` |
| 1 | Gap analysis: ALERT, OPS, GOV | B | `03-analysis/findings/ALERT_findings.md`, `OPS_findings.md`, `GOV_findings.md` |
| 2 | Gap analysis: SEC (high-level), MULTI (lightweight) | B | Remaining findings files |
| 2 | Quick wins identification: Critical/High gaps fixable in < 1 week | Both | `04-recommendations/quick-wins.md` |
| 3 | **Workshop #3 — Leadership** (if required by client) | Both | Raw notes |
| 3 | Target observability architecture draft — current state, target state, governance model | Both | `05-deliverables/target-architecture.md` (draft) |
| 4 | **Interim findings review with client** — share top Critical/High findings; validate quick wins | Both | Client-validated findings |
| 5 | Incorporate client feedback; finalise gap register | Both | `03-analysis/gap-register.md` |

**Must have by end of Week 3:**
- [ ] All in-scope domain findings written and peer-reviewed
- [ ] Gap register complete with severity, effort, quick win flag for all findings
- [ ] Quick wins list ready to hand over — client can start acting immediately
- [ ] Client has seen top findings (no surprises at final delivery)
- [ ] Target architecture draft covers current state, target state, and migration path

**Workshop #3 focus (Leadership):**
- What SLA commitments exist with consumers — contractual obligations, penalties?
- What is the business impact of a consumer-reported incident vs. one detected internally?
- What does "good observability" mean to you in 6–12 months?
- Are there compliance or regulatory requirements we should factor into the architecture?

---

### Week 4 — Recommendations & Roadmap

**Goal:** Finalise all deliverables. Produce the target architecture document, technical report, executive summary, roadmap, and governance guidelines.

| Day | Activity | Specialist | Output |
|---|---|---|---|
| 1 | Target observability architecture — finalise (current state, target state, gap narrative) | Both | `05-deliverables/target-architecture.md` |
| 1–2 | Technical assessment report — all domain findings, gap summary, maturity scores | Both | `05-deliverables/technical-report.md` |
| 2 | Improvement roadmap — phased plan with effort estimates | B | `04-recommendations/roadmap.md` |
| 3 | Governance guidelines — principles to leave with the team | B | `05-deliverables/governance-guidelines.md` |
| 3 | Executive summary | A | `05-deliverables/executive-summary.md` |
| 4 | Internal review and QA — verify every Critical finding traces to a deliverable | Both | Final documents |
| 5 | **Final delivery presentation** | Both | Presented to client |

---

## Deliverables

| Deliverable | Path | Owner | Due |
|---|---|---|---|
| Technical assessment report | `05-deliverables/technical-report.md` | Both | Week 4, Day 2 |
| Target observability architecture | `05-deliverables/target-architecture.md` | Both | Week 4, Day 1 |
| Executive summary | `05-deliverables/executive-summary.md` | A | Week 4, Day 3 |
| Improvement roadmap + quick wins | `04-recommendations/roadmap.md` | B | Week 4, Day 2 |
| Governance guidelines | `05-deliverables/governance-guidelines.md` | A | Week 4, Day 3 |

---

## Domain Priority Order for Gap Analysis

When running gap-analyst, analyse in this order (most critical first):

1. **ALERT** — primary pain point; highest business impact if wrong
2. **BAM** — business activity monitoring; consumer KPIs; SLA compliance tracking; business metrics in Datadog
3. **INSTR** — SLA/SLO instrumentation; are the right signals being emitted?
4. **COLL** — log pipeline depth; retention; coverage gaps per tier (includes AKS Container Insights and VM agent coverage)
5. **OPS** — incident response process; runbook maturity; on-call workflow
6. **GOV** — governance model; tag strategy; monitor ownership; RBAC
7. **SEC** — high-level only; WAF, audit, compliance
8. **MULTI** — lightweight; tag-based environment isolation in single Datadog tenant

AUTO, DD, APM, DASH, and AKS (as a standalone domain) are out of scope for Phase 1. AKS infrastructure monitoring evidence feeds COLL findings. Do not write AKS-domain findings — record AKS gaps under COLL instead.

---

## Critical Dependencies

| Dependency | Owner | Needed by | Risk if late |
|---|---|---|---|
| Datadog API access (read-only) | Client | Week 1, Day 1 | Entire Datadog analysis blocked |
| Azure read access (all subscriptions) | Client | Week 1, Day 1 | Azure infra analysis blocked |
| SLA/SLO definitions document | Client | Week 2, Day 3 | INSTR/SLO gap analysis incomplete |
| Workshop participants confirmed | Client | End of Week 1 | Workshops slip → Week 3 compressed |

---

## Key Open Questions (from introductory meeting)

1. DB crash: Was it detected by monitoring or by consumers? MTTD?
2. What makes alerting painful — noise, routing, missing coverage?
3. Is there a log archive beyond the 30-day Datadog online index?
4. How are consumer environments tagged/isolated in the single Datadog tenant?
5. CDN provider — Azure CDN, Cloudflare, other?
6. What bots and automation are currently in place?
7. PagerDuty / OpsGenie — which tool for on-call routing?
8. Any compliance or regulatory requirements (SOC 2, ISO 27001, GDPR)?
