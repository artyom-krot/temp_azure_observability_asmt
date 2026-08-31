# Kick-off Session — Observability Assessment
**Audience:** Client project sponsor, technical lead, operations lead  
**Format:** Remote / 90 minutes  
**Prepared by:** Supplier engagement team  
**Status:** Draft — for internal prep

---

## Session Objectives

By the end of this session, both parties should have:

- A shared understanding of what we know about the platform today and where the knowledge gaps are
- Agreement on the problem statement we are solving
- A confirmed assessment scope for Phase 1
- Shared understanding of expected outcomes and deliverables
- An agreed high-level plan and timeline
- Acknowledged risks, assumptions, and dependencies — with owners assigned

---

## Agenda

| # | Topic | Time |
|---|---|---|
| 1 | What we know right now | 10 min |
| 2 | Problem statement | 10 min |
| 3 | Scope definition | 20 min |
| 4 | Expected outcomes | 10 min |
| 5 | Assessment plan | 15 min |
| 6 | RAID — risks, assumptions, dependencies | 15 min |
| — | Open discussion / next steps | 10 min |

---

## 1. What We Know Right Now

### Platform Overview
The Client operates a custom Java application delivered as a **dedicated hosted instance per enterprise consumer**. Consumers span four active regions — US, UK, Australia — with Germany in progress and Canada planned. This means the platform is not a single monolith but a collection of production environments, currently **12 in total** (primary + DR per region), each serving a distinct enterprise client.

Multiple product teams share the same platform, each owning specific services or integrations.

### Infrastructure Landscape

| Tier | Component | Notes |
|---|---|---|
| Edge | CDN | Provider TBC (Azure CDN or Cloudflare) |
| Proxy | Apache | Web / reverse proxy layer |
| Application (modernised) | Azure Kubernetes Service (AKS) | Namespace-based environment isolation |
| Application (legacy) | Azure Virtual Machines | Migration to AKS in progress |
| Messaging | Azure Service Bus | Regional async messaging between services |
| Storage | Azure Blob Storage | Document/content storage, geo-redundant |
| Load balancing / WAF | Azure Application Gateway + WAF | L7 load balancing, web application firewall |
| Database | Azure SQL Database | Managed relational DB with HA |
| Secrets | Azure Key Vault | Secrets and certificate management |
| Platform observability | Azure Monitor + Log Analytics | Infrastructure-level signals |
| Primary monitoring | Datadog | All consumer environments in a single tenant |

### Monitoring Context
- The Client migrated from Splunk to Datadog approximately two years ago.
- Datadog is the **primary monitoring platform**, but operates as a **single tenant across all consumer environments**. Environment separation relies entirely on tag-based isolation (`env`, `consumer`, `service`).
- Azure Monitor and Log Analytics provide infrastructure-level signals alongside Datadog.

### What We Don't Know Yet *(to confirm in this session)*
- Full inventory of services, integrations, and environments in scope
- Current instrumentation coverage per tier
- Whether all 12 environments are consistently configured
- Whether any environments are "dark" (no monitoring signals)
- Incident history beyond the confirmed database crash
- Existing SLA/SLO definitions and whether they map to observable signals

---

## 2. Problem Statement

### Current State
The Client's observability posture is **reactive**. Failures in the production platform are being detected and reported by consumers before internal monitoring raises an alert. This is confirmed by a recent database crash that caused a consumer-facing outage — an event that monitoring should have detected first.

This pattern creates direct reputational and contractual risk: consumers experience degraded service, report it externally, and the Client's operations team responds to a problem they did not anticipate.

### Root Causes (Hypothesised — to validate during assessment)
- Instrumentation gaps: not all platform tiers emit sufficient signals to support early detection
- Alert coverage gaps: critical failure modes are not represented as actionable monitors
- Alert quality: alerts may be noisy, poorly tuned, or routed to the wrong teams
- SLA/SLO disconnect: contractual obligations are not mapped to measurable, automated observability signals
- Governance gaps: no consistent standard for what must be monitored, owned by whom, or reviewed how

### Ideal Long-Term State *(the north star)*
A mature, production-grade observability practice where:
- Platform health is **continuously and proactively monitored** across all tiers and all consumer environments
- Every contractual SLA obligation is backed by an **automated observability signal** that alerts before a breach occurs
- Incidents are detected **internally**, with MTTD measured in minutes — not hours reported by consumers
- Engineering, operations, and leadership each have **purpose-built visibility** into the health of the platform
- Observability standards are **governed, owned, and consistently applied** across all environments and all teams

---

## 3. Scope Definition

### Platform Scope *(proposed — confirm with Client)*

| Layer | In scope | Notes |
|---|---|---|
| Azure infrastructure (AKS, VMs, SQL, Service Bus, Blob, App Gateway, Key Vault) | Yes | All active regions |
| Apache web tier | Yes | |
| CDN edge layer | Yes | Provider TBC |
| Datadog monitoring platform | Yes | Monitors, dashboards, SLOs, APM, log pipelines |
| Azure Monitor + Log Analytics | Yes | Diagnostic settings, metric coverage |
| Application source code (instrumentation review) | Yes | Read-only, selected repositories |
| CI/CD pipelines (observability-as-code review) | Yes | View-only |
| Non-production environment (runtime inspection) | Yes | Dev or staging only |
| DE environment (in progress) | TBC | May be too early-stage for inclusion |
| CA environment (planned) | No | Not yet active |
| Business activity monitoring / consumer KPIs | No | Deferred to Phase 2 |

**Active regions in scope:** US, UK, AU (DE to confirm)

### People and Capacity
- **Supplier:** 2 specialists — Lead Engineer (instrumentation, APM, log pipelines, AKS) and Systems Architect (alerting, governance, operations, target architecture)
- **Timeline:** 4 weeks of active delivery
- This scope is **calibrated to 2 people over 4 weeks** — not all domains can receive equal depth; priorities are agreed below

### Current Pain Points — Priority Focus Areas
Based on what is known today, the following are the highest-priority areas for this engagement:

1. **Alerting and detection gaps** — the reactive detection posture is the confirmed, active problem
2. **Instrumentation coverage** — determining which tiers are dark and why
3. **SLA/SLO observability** — whether contractual obligations have any automated backing
4. **APM depth** — whether root cause analysis is currently possible when incidents occur
5. **Governance** — whether any standards exist and are being followed

### Out of Scope — Phase 2 Candidates
The following are acknowledged as important but are not included in this engagement:

- Multi-region consistency analysis at depth
- Datadog platform utilisation optimisation
- Monitoring-as-code / CI/CD automation for observability
- Business activity monitoring and consumer KPI dashboards
- Unified observability architecture implementation (Phase 2 executes the Phase 1 design)

---

## 4. Expected Outcomes

### Deliverables

| Deliverable | Audience | Description |
|---|---|---|
| Technical Assessment Report | Engineering & Architecture | Evidence-backed gap analysis across all assessed domains; current-state findings |
| Target Observability Architecture | Engineering & Architecture | Blueprint for what mature observability looks like for this platform |
| Improvement Roadmap | Engineering & Leadership | Prioritised action plan; quick wins explicitly called out |
| Governance Guidelines | Engineering & Operations | Principles and ownership model to leave with the team |
| Executive Summary | Leadership & Stakeholders | Non-technical summary of current state, key risks, and recommended path |

### What "Good" Looks Like at the End of 4 Weeks

**Gap analysis output:**
- Clear mapping of which tiers and failure modes are monitored vs. not
- Severity-rated findings with evidence and specific recommendations
- Identification of blind spots where consumer reports would precede internal detection

**Roadmap output:**
- Short-term quick wins (actionable within 2–4 weeks post-assessment, minimal effort)
- Long-term strategic improvements with effort and priority guidance

**Issues identified in two dimensions:**
- *Technical gaps* — specific missing monitors, instrumentation, dashboards, or configurations
- *Cultural and process gaps* — missing ownership, governance, or operational practices that, without addressing, will cause technical gaps to recur regardless of tooling improvements

---

## 5. High-Level Assessment Plan

### Week-by-Week Approach

| Week | Focus | Key activities |
|---|---|---|
| **Week 1** | Discovery & Landscape | Kick-off; Engineering Workshop; landscape review; access provisioning; initial evidence collection |
| **Week 2** | Technical Analysis | Operations Workshop; deep-dive into Datadog (monitors, APM, logs, SLOs); Azure Monitor review; AKS instrumentation; source code review |
| **Week 3** | Gap Analysis | Structured gap analysis across all domains; findings drafting; Interim Findings Review with Client |
| **Week 4** | Recommendations & Delivery | Target architecture; roadmap; report finalisation; Leadership Workshop (if applicable); Final Presentation |

### Proposed Workshops Plan

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

### Toolset Used in This Assessment

| Tool | Purpose |
|---|---|
| Azure CLI + Azure Portal | Live inspection of diagnostic settings, Azure Monitor, Log Analytics, AKS |
| Datadog API (read-only) | Programmatic export and analysis of monitors, dashboards, SLOs, APM config, log indexes |
| Datadog UI (Viewer role) | Direct inspection and screenshot evidence |
| Source code (read-only) | Instrumentation verification — agent setup, logging framework, custom metrics |
| **Claude (Anthropic AI)** | Evidence analysis, gap identification, report drafting (see AI disclosure below) |

**AI Tooling Disclosure:** This engagement uses Claude, an AI language model, to assist with evidence analysis, gap identification, and report drafting. The Supplier's specialists retain full responsibility for all findings and recommendations. No production log data or PII is processed through AI tools — only configuration exports and structural metadata. Client should confirm this is acceptable before Week 1 begins.

---

## 6. RAID — Risks, Assumptions, Issues, Dependencies

### Risks

| # | Risk | Impact | Owner |
|---|---|---|---|
| R1 | Azure/Datadog access not provisioned by Week 1 Day 1 | Timeline compresses or slips | Client |
| R2 | Key workshop participants unavailable or substituted with proxies | Findings based on assumptions, reduced accuracy | Client |
| R3 | SLA/SLO definitions, architecture docs, or incident records unavailable | Gap analysis lacks contractual grounding; findings may understate severity | Client |
| R4 | Platform scope is larger than currently known — services discovered in Week 1 that are not in this SOW | Critical gaps in undisclosed components go unassessed | Both |
| R5 | Security/IAM policies block full read-only access to required systems | Affected domains have incomplete coverage | Client |
| R6 | Single Datadog tenant complexity — tag-based isolation makes per-environment signal analysis difficult | Environment-specific gaps may go undetected | Client / Supplier |
| R7 | Client data handling policy incompatible with AI-assisted analysis tooling | Methodology must be revised; timeline extends | Both |
| R8 | Delays in Weeks 1–2 compress analysis and delivery weeks | Report and architecture quality reduced | Both |

### Assumptions *(to confirm in this session)*

- The Client operates exactly 12 production environments across US, UK, and AU at time of engagement start
- Datadog is the single monitoring platform for all environments; no parallel or shadow monitoring tools exist
- The AKS-to-VM migration is in progress but not complete — both tiers are in scope
- The Client has existing SLA commitments with consumers, even if they are not currently instrumented as SLOs
- At least one designated technical point of contact is available throughout the 4-week engagement
- Week 1 landscape discovery may surface additional components; both parties agree to handle scope additions via written agreement

### Dependencies

| Dependency | Blocking? | Owner | Required by |
|---|---|---|---|
| Azure read-only access (Reader, Log Analytics Reader, AKS Cluster User, Policy Reader) | **Yes** | Client | Week 1, Day 1 |
| Datadog API Key + Application Key (read-only) | **Yes** | Client | Week 1, Day 1 |
| Datadog Viewer role (UI access) | **Yes** | Client | Week 1, Day 1 |
| Source code repository read access | **Yes** | Client | Week 1 |
| Dev/staging environment access for runtime inspection | Yes | Client | Week 2 |
| CI/CD pipeline view access | Yes | Client | Week 2 |
| Named Engineering Workshop participants confirmed | **Yes** | Client | Before Week 1 ends |
| Named Operations Workshop participants confirmed | **Yes** | Client | Before Week 2 |
| Architecture diagram / infrastructure documentation | Yes | Client | Week 1 |
| SLA/SLO definitions per consumer or service tier | Yes | Client | Week 2 |
| Incident post-mortems (incl. recent database crash) | Yes | Client | Week 2 |
| AI tooling acceptable-use confirmation | **Yes** | Client | Before Week 1 |
| In-scope subscription IDs list | **Yes** | Client | Kick-off |

---

## Next Steps (to close in this session)

- [ ] Client confirms or adjusts platform scope (tiers, regions, environments)
- [ ] Client confirms AI tooling acceptable-use policy
- [ ] Client nominates workshop participants for Engineering and Operations sessions
- [ ] Client names a single point of contact for access provisioning
- [ ] Supplier and Client agree access provisioning deadline (target: 2 business days post kick-off)
- [ ] Client provides in-scope subscription ID list
- [ ] Both parties confirm Week 1 Engineering Workshop date/time
