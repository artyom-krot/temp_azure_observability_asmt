# Observability Assessment — Engagement Scope
**Status:** Draft — for client discussion  
**Prepared by:** Supplier engagement team  
**Engagement duration:** 4 weeks  
**Team:** Lead Engineer + Systems Architect

---

## Purpose

The Supplier will conduct a structured assessment of the Client's observability posture across the Azure-hosted application platform. The engagement is led by a team of two specialists — a Lead Engineer and a Systems Architect — and is scoped to four (4) weeks of active delivery.

The primary outcome of this engagement is a documented picture of what a mature, production-grade observability practice looks like for this platform, an objective assessment of the current state against that standard, and a prioritised, actionable roadmap to close identified gaps.

---

## In-Scope Activities

### 1. Application and Infrastructure Landscape Review
A structured review of the application's hosting environment, service tiers, and integration points. This activity establishes the factual baseline — what components exist, how they are deployed, and which are subject to monitoring. Without this, any observability gap analysis would be incomplete or inaccurate.

### 2. Observability Coverage Evaluation
Review of log collection pipelines, metric emission coverage, and tracing instrumentation across all in-scope tiers. The assessment will identify which tiers are monitored, which are dark (producing no signals), and where gaps in instrumentation create operational blind spots that prevent early detection of failures.

### 3. Alerting and Detection Gap Analysis
Structured review of the alert inventory against Critical and High-severity failure modes, including an assessment of alert quality indicators and recurring incident patterns. This activity will document specific gaps where consumer-reported incidents would be detected before internal monitoring — directly addressing the reactive detection posture identified as the core problem.

### 4. SLA/SLO Observability Review
Identification and mapping of the Client's contractual SLA commitments to existing SLO monitors. This activity will surface gaps where SLA obligations are not represented as measurable, automated observability signals — creating risk that breaches go undetected until reported externally.

### 5. Operational Processes Review
Assessment of the end-to-end incident detection and response workflow, covering alert routing, on-call tooling, runbook availability and quality, and escalation paths. This activity will evaluate the Client's MTTD (Mean Time to Detect) and MTTR (Mean Time to Recover) posture using documented incidents as evidence.

### 6. Governance Model Review
Assessment of current-state observability governance maturity. This covers how monitoring standards are defined, who owns alerting and instrumentation decisions, how access to monitoring data is controlled, and whether governance practices are consistent across teams and environments.

---

## Deliverables

| # | Deliverable | Audience | Description |
|---|---|---|---|
| 1 | Technical Assessment Report | Engineering & Architecture | Detailed current-state findings, evidence-backed gaps, and domain-level analysis |
| 2 | Target Observability Architecture | Engineering & Architecture | Blueprint for a mature, production-grade observability practice for this platform |
| 3 | Improvement Roadmap | Engineering & Leadership | Prioritised list of actions with quick wins called out explicitly |
| 4 | Governance Guidelines | Engineering & Operations | Principles and ownership model to leave with the team |
| 5 | Executive Summary | Leadership & Stakeholders | Non-technical summary of current state, key risks, and recommended path forward |

---

## Out of Scope — Deferred to Phase 2

The following areas are acknowledged as important but are not included in this engagement. They are recommended as direct inputs to Phase 2 planning.

| Deferred area | Reason for deferral |
|---|---|
| Application Performance Monitoring (APM) — distributed tracing, dependency visibility, root-cause analysis capabilities | Deferred to maintain Phase 1 focus on detection and coverage gaps |
| Dashboard & Visibility Assessment — audience-specific dashboards (engineering, operations, leadership) | Deferred; depends on coverage and alerting gaps being addressed first |
| Monitoring-as-code and CI/CD integration for observability | Requires Phase 1 baseline to be established first |
| Multi-region consistency and coverage | Dependent on Phase 1 findings; scope would expand engagement significantly |
| Datadog platform utilisation optimisation | Tactical improvement; best actioned after strategic gaps are addressed |
| Business activity monitoring (consumer KPIs, SLA compliance dashboards) | High value but requires business requirements alignment beyond Phase 1 scope |
| Unified observability architecture implementation | Phase 2 execution work; Phase 1 produces the design |
| Standardisation at scale / migration strategy | Follows directly from Phase 1 target architecture |

---

## Client Dependencies

The following must be provided by the Client for successful delivery. Items marked **[Blocking]** will delay engagement progress if not available on time.

---

### 1. Workshops and Working Sessions

| Session | Participants | Notes |
|---|---|---|
| Kick-off meeting | Client project sponsor, technical lead, operations lead | **[Blocking]** — required before analysis begins |
| Engineering Workshop | 3–5 engineers with first-hand knowledge of the application stack, infrastructure, and log pipelines | **[Blocking]** |
| Operations / On-call Workshop | On-call engineers and incident response team | **[Blocking]** |
| Leadership Workshop *(conditional)* | Engineering Director or equivalent | Conditional on leadership availability |
| Interim Findings Review | Technical lead and one decision-maker | 60 min, remote |
| Final Delivery Presentation | Full stakeholder group (technical and leadership) | |

---

### 2. Platform Access

All access is **read-only**. No write or modify permissions are required or should be granted.

#### Azure
**[Blocking]** — required before infrastructure analysis begins.

| Role / Permission | Scope |
|---|---|
| Reader | All in-scope Azure subscriptions |
| Log Analytics Reader | All in-scope Log Analytics workspaces |
| Azure Kubernetes Service Cluster User | All in-scope AKS clusters |
| Azure Policy Reader | Subscription or management group level |
| Azure Monitor Reader | All in-scope subscriptions |

The Client must provide a list of in-scope subscription IDs at kick-off. Access should be provisioned via a dedicated service principal or named accounts with MFA.

#### Datadog
**[Blocking]** — UI-only access is insufficient. API credentials are required for full analysis.

| Credential / Role | Purpose |
|---|---|
| Read-only API Key + Application Key | Programmatic export of monitors, dashboards, SLOs, log indexes, and agent inventory |
| Datadog Viewer role | UI access |

#### Source Code Repositories
Read-only access to application service repositories — **TBC; confirm at kick-off**. Required to verify instrumentation configuration (agent setup, logging framework, custom metric emission) if access can be provisioned.

#### Application Environment
Read-only access to a non-production (dev or staging) environment for runtime inspection of agent configuration and log output.

#### CI/CD Pipelines
View access to CI/CD pipeline definitions — required for evidence review of current instrumentation practices. Note: observability automation and monitoring-as-code are Phase 2 scope; this access is for passive review only.

---

### 3. Documentation and Reference Materials

| Material | Purpose |
|---|---|
| Current-state architecture diagram (or best available approximation) | Scope baseline |
| SLA and SLO definitions — contractual obligations per consumer or service tier | SLO gap analysis |
| Incident post-mortems or retrospectives for significant events (including the recent database crash) | Detection gap analysis |
| Existing runbooks and alerting playbooks, if any | Operational maturity assessment |
| Any prior observability or monitoring review documentation | Baseline context |
| Infrastructure inventory or Terraform state export, if available | Coverage mapping |

---

### 4. Technical Disclosure — AI-Assisted Analysis

This engagement uses Claude (Anthropic), an AI language model, as a tool to assist in evidence analysis, gap identification, and report drafting. The Supplier's specialists retain full responsibility for all findings and recommendations. The Client should confirm that use of AI tooling for internal analysis is consistent with their data handling and acceptable-use policies prior to engagement commencement.

---

## Risks

The following risks have been identified as relevant to this engagement. Both parties are responsible for monitoring and mitigating risks within their respective control.

---

### Risk 1 — Incomplete Scope Identification at Engagement Start
**Risk:** The full application and infrastructure landscape is not completely known at the time this SOW is signed. Additional services, integrations, environments, or infrastructure tiers may be discovered during Week 1 discovery and workshops that were not visible at scoping time.  
**Impact:** The agreed assessment scope may not cover the actual platform in full. Critical observability gaps in undisclosed or unknown components may go unassessed, creating blind spots in the findings and target architecture.  
**Mitigation:** Week 1 is explicitly structured as a landscape discovery phase to surface the complete platform tier map. Any material components identified during discovery that fall outside the agreed scope will be flagged to the Client immediately. Both parties will agree in writing whether to include them within the current engagement or defer to Phase 2. The Supplier cannot be held responsible for gaps in components not disclosed or discoverable within the agreed timeline.  
**Owner:** Both

---

### Risk 2 — AI Tooling Data Handling
**Risk:** Client data (configuration exports, alert content, log samples) processed through AI-assisted analysis tools does not meet the Client's data classification or acceptable-use policy requirements.  
**Impact:** Engagement methodology must be revised mid-delivery; re-analysis without AI tooling extends timeline.  
**Mitigation:** Client confirms acceptability of AI-assisted analysis tooling before engagement commencement. No production log data or PII is fed into AI tools — only configuration exports and structural metadata.  
**Owner:** Both

---

### Risk 3 — Timeline Compression in Weeks 3–4
**Risk:** Delays accumulated in Weeks 1–2 (access, workshops, documentation) compress the analysis and delivery weeks.  
**Impact:** Gap analysis is rushed; target architecture and report quality are reduced.  
**Mitigation:** Supplier flags timeline risk to the Client no later than end of Week 2. If more than 3 business days are lost to Client-side delays, the delivery date extends by equivalent time.  
**Owner:** Both
