# Azure Observability Assessment — Project Brain

This file is the persistent context for Claude across all sessions in this project.
Read this at the start of every session. All analysis, findings, and recommendations must
align with the scope, standards, and format defined here.

---

## Client Context

- **Client type:** Large enterprise ISV
- **Product:** Custom Java application, deployed as a **dedicated instance per consumer** (enterprise companies)
- **Consumers:** Global enterprise clients across US, UK, AU, DE (WIP), CA (planned)
- **Teams:** Multiple product teams sharing a single platform, each owning specific services or integrations
- **Core problem:** Observability is **reactive** — consumers detect failures before monitoring does
- **Confirmed incident:** Recent database crash caused a consumer-facing outage — evidence of reactive detection posture
- **Monitoring history:** Migrated from Splunk to Datadog ~2 years ago

### Infrastructure Landscape

| Resource | Role |
|---|---|
| CDN | Edge layer (provider TBC — Azure CDN / Cloudflare) |
| Apache | Web/reverse proxy tier |
| Azure VMs | Legacy application components (migration in progress) |
| Azure Kubernetes Service (AKS) | Modernized microservices, namespace-based env isolation |
| Azure Service Bus (ASB) | Regional async messaging between services |
| Azure Blob Storage | Document/content storage, geo-redundant |
| Azure Application Gateway + WAF | L7 load balancing, web application firewall |
| Azure SQL Database | Managed relational DB with HA |
| Azure Key Vault | Secrets and certificate management |
| Azure Monitor + Log Analytics | Platform-level observability |
| Datadog | **Primary monitoring platform — single tenant for ALL consumer environments** |

### Datadog Tenancy Model — Critical Context
**Single Datadog tenant** is used for all consumer environments (not per-consumer tenants).
Tag-based isolation (`env`, `consumer`, `service`) is the only separation mechanism.
This makes tag governance, RBAC, and UST compliance critical architectural concerns.

### Scale
- **12 production environments** (primary + DR per region)
- **Regions active:** US, UK, AU — DE in progress, CA planned

### Engagement Team
- **2 specialists** on this assessment. Scope must be focused accordingly — not all 12 domains can receive equal depth in 4 weeks.
- **Specialist A (Lead Engineer):** Responsible for application infrastructure landscape, application performance monitoring, log collection, and technical capabilities. Brings hands-on expertise to assess instrumentation, log pipelines, and APM agent coverage firsthand.
- **Specialist B (Systems Architect):** Responsible for governance model, operational strategy, observability coverage, recurring issues, and assessment approach. Owns the alerting architecture, governance model, operational process design, and structural layers of the target architecture. Note: AKS infrastructure monitoring evidence is gathered by azure-analyst and feeds COLL findings.

---

## Engagement Scope

**Team:** 2 specialists. **Timeline:** 4 weeks.

### Ultimate Outcome
The primary deliverable is a **target observability architecture** — not just a list of gaps.
The output must answer: "Here is what good observability looks like for your platform, here is how far you are today, and here is the prioritised path to get there." The architecture must account for standardised instrumentation, governance, and operational practices that scale as the platform continues its cloud-native transition.

### In-Scope Activities (Weeks 1–4)

| # | Activity | Primary domains |
|---|---|---|
| 1 | **Application & Infrastructure Landscape Review** — identify all in-scope tiers, services, and components | INSTR, COLL |
| 2 | **Observability Coverage Evaluation** — log pipelines, metric emission, tracing across all tiers; identify dark tiers and instrumentation blind spots | INSTR, COLL |
| 3 | **Alerting & Detection Gap Analysis** — alert inventory coverage vs. Critical/High failure modes; alert quality; recurring incidents; gaps where consumers detect before monitoring does | ALERT |
| 4 | **SLA/SLO Observability Review** — map contractual SLA commitments to existing SLO monitors; identify gaps where SLA obligations lack automated observability signals | INSTR, ALERT, OPS |
| 5 | **Operational Processes Review** — assess end-to-end incident detection and response: alert routing, on-call tooling, runbook quality, escalation paths; evaluate MTTD/MTTR posture | OPS |
| 6 | **Governance Model Review** — assess current-state governance maturity across tagging, ownership, access control, and standards adoption | GOV |

### Out of Scope — Deferred to Phase 2

These are acknowledged as important but not covered in this engagement. They should feed directly into Phase 2 planning.

| Deferred topic | Domain |
|---|---|
| Application Performance Monitoring (APM) — distributed tracing, dependency visibility, root-cause analysis capabilities | `APM` |
| Dashboard & Visibility Assessment — audience-specific dashboard coverage (engineering, operations, leadership) | `DASH` |
| Datadog capability utilisation optimisation | `DD` |
| Monitoring-as-code / CI/CD automation | `AUTO` |
| Unified monitoring architecture design | `MULTI` |
| Standardise observability: guardrails, ownership, unified toolset | `GOV` (standardise) |
| Apply at scale / Migration strategy unified observability | N/A |
| Multi-region coverage | `MULTI` |
| Business activity monitoring — consumer KPIs, SLA compliance, business metrics | `BAM` |

### Workshop Plan

| # | Session | Priority | Timing | Main Purpose | Expected Participants |
|---|---|---|---|---|---|
| 1 | Kick-off & Scope Alignment | Mandatory | Week 1 | Confirm problem statement, Phase 1 scope, priorities, deliverables, dependencies, access, RAID | Sponsor, Product/Platform leadership, Architect, Ops/SRE lead |
| 2 | Architecture & Engineering Workshop | Mandatory | Week 1 | Validate application/infrastructure landscape, critical services, dependencies, regions, modernization context | Platform architects, Cloud/AKS engineers, App leads |
| 3 | Observability Azure Monitor / Datadog Deep Dive | Mandatory | Week 1 | Review telemetry model, Datadog usage, logs/metrics/traces, APM, monitors, dashboards, integrations, governance | Datadog/Observability owner, SRE/Ops |
| 4 | Operations, Incidents & SLA/SLO Workshop | Mandatory | Week 2 | Review incident lifecycle, detection gaps, alerting, MTTD/MTTR, SLA/SLO monitoring, known pain points | Operations/Support, SRE, Service owners, SLA/SLO owners |
| 5 | Application Observability Deep Dive | Recommended | Week 2 | Review representative critical service instrumentation, tracing, dependencies, code-level observability | App engineers, Tech leads |
| 6 | Governance & Operating Model Workshop | Recommended | Week 2–3 | Review ownership, standards, onboarding, access, lifecycle, consistency across teams | Platform/Ops leads, Architecture, Governance/Security |
| 7 | Interim Findings Review | Mandatory | Week 3 | Validate findings, correct assumptions, agree priorities before final recommendations | Key SMEs + sponsor |
| 8 | Final Findings & Roadmap Presentation | Mandatory | Week 4 | Present gaps, recommendations, quick wins, estimates and roadmap | Leadership + key technical stakeholders |

### Deliverables

| # | Deliverable | Path |
|---|---|---|
| 1 | Technical assessment report (gaps + current state) | `05-deliverables/technical-report.md` |
| 2 | Target observability architecture | `05-deliverables/target-architecture.md` |
| 3 | Executive summary | `05-deliverables/executive-summary.md` |
| 4 | Improvement roadmap with quick wins | `04-recommendations/roadmap.md` |
| 5 | Governance guidelines (principles to leave with the team) | `05-deliverables/governance-guidelines.md` |

---

## Assessment Domains

All findings reference one of the 12 standard domains. Scope column indicates depth for this engagement.

| # | Domain | Code | Scope |
|---|--------|------|-------|
| 1 | Instrumentation | `INSTR` | **Full** |
| 2 | Collection & Pipelines | `COLL` | **Full** |
| 3 | Alerting | `ALERT` | **Full** |
| 4 | Dashboards & Visibility | `DASH` | **Deferred to Phase 2** |
| 5 | Business Activity Monitoring (BAM). Is the application producing the correct business outcomes? | `BAM` | Lightweight |
| 6 | Multi-Environment Consistency | `MULTI` | Lightweight |
| 7 | APM (Application Performance Monitoring) & Distributed Tracing | `APM` | **Deferred to Phase 2** |
| 8 | Security Observability | `SEC` | High-level only |
| 9 | Operational Processes | `OPS` | **Full** |
| 10 | Governance | `GOV` | **Full** (current state; not standardisation) |
| 11 | Observability Automation & CI/CD | `AUTO` | **Out of scope** |
| 12 | Datadog Platform Utilization | `DD` | **Out of scope** |

---

## Standards & Frameworks Applied

All gap analysis and recommendations must be grounded in these:

- **Google SRE principles** — SLO-driven alerting, symptom-based vs. cause-based alerts, error budgets
- **OpenTelemetry specification** — instrumentation completeness (logs, metrics, traces)
- **Azure Well-Architected Framework** — Operational Excellence pillar
- **DORA metrics** — MTTD (Mean Time to Detect) and MTTR (Mean Time to Recover) as outcome measures
- **Datadog maturity model** — coverage of Datadog modules (Infrastructure, APM, Logs, Synthetics, DBM, SIEM)
- **CNCF Observability whitepaper** — structured maturity scoring

---

## Finding Format

Every finding written to `03-analysis/findings/` must follow this exact structure:

```markdown
### [DOMAIN-###] Finding Title

| Field | Value |
|---|---|
| Domain | DOMAIN_CODE — Domain Name |
| Severity | Critical / High / Medium / Low |
| Evidence | What was observed (link to discovery file or quote) |
| Gap | What is missing or misconfigured |
| Impact | What risk or consequence this creates |
| Recommendation | What should be done |
| Effort | Low / Medium / High |
| Quick Win | Yes / No |
```

**Severity definitions:**
- **Critical** — Active blind spot; production incidents go undetected
- **High** — Material gap; increases MTTD/MTTR significantly
- **Medium** — Best practice gap; manageable risk but accumulates technical debt
- **Low** — Minor improvement; polish and efficiency

---

## Gap Register

All findings must also be logged in `03-analysis/gap-register.md` as a single-line table row.

Format:
```
| ID | Domain | Title | Severity | Quick Win | Status |
```

---

## Repo Structure

```
00-context/        Client-provided materials (architecture docs, exports, workshop notes)
01-framework/      Assessment checklist, maturity model, interview questionnaires
02-discovery/      Raw evidence per domain (Azure Monitor configs, Datadog exports, etc.)
03-analysis/       Gap analysis findings and gap register
04-recommendations/ Improvement plan (quick wins, roadmap, implementation plan)
05-deliverables/   Final reports and executive presentation
.claude/agents/    Specialized subagent definitions
```

---

## Behavioral Instructions for Claude

1. **Every analysis session:** Identify which domain(s) you are working in and state it explicitly.
2. **When analyzing evidence:** Always compare against the standards listed above. Note which standard is violated.
3. **When writing findings:** Use the finding format template. Never write a finding without evidence.
4. **When writing recommendations:** Be specific — name the Azure resource, Datadog feature, or config change. Avoid generic advice.
5. **Multi-environment sensitivity:** Always ask — "does this apply to all 12 environments or just some?" Configuration drift between envs is a primary finding type.
6. **CI/CD angle:** For every technical gap found, consider whether it would be prevented by automation (domain 11). If yes, note it.
7. **Business metrics gap:** When metrics coverage is discussed, explicitly check whether business-level metrics exist (e.g., per-consumer transaction rates, SLA tracking). This is typically missing and is a high-value finding.
8. **Datadog license utilization:** When reviewing Datadog, always map which modules are licensed vs. which are actively configured. Gap between the two is a finding.
9. **Documentation hygiene (proactive, no prompt needed):** After any analysis, evidence ingestion, workshop processing, or structural change to the engagement — automatically check whether any of the following need updating and update them without waiting to be asked:
   - `CLAUDE.md` — new client context, scope changes, new standards, revised domain definitions
   - `HOW-TO-USE.md` — new workflow steps, new agents, new example prompts, corrected instructions
   - `.claude/agents/*.md` — agent instructions that are incomplete or inconsistent with new evidence
   - `01-framework/` — checklist items or questionnaire gaps surfaced by real client data
   Do this silently (just make the edits) unless the change is substantial enough to warrant a brief note to the user.
