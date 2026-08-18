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

### Infrastructure Landscape

| Resource | Role |
|---|---|
| Azure VMs | Legacy application components (migration in progress) |
| Azure Kubernetes Service (AKS) | Modernized microservices, namespace-based env isolation |
| Azure Service Bus (ASB) | Regional async messaging between services |
| Azure Blob Storage | Document/content storage, geo-redundant |
| Azure Application Gateway + WAF | L7 load balancing, web application firewall |
| Azure SQL Database | Managed relational DB with HA |
| Azure Key Vault | Secrets and certificate management |
| Azure Monitor + Log Analytics | Platform-level observability |
| Datadog | Primary monitoring platform |

### Scale
- **12 production environments** (primary + DR per region)
- **Regions active:** US, UK, AU — DE in progress, CA planned

---

## Engagement Scope

This is a **full observability architectural assessment** covering both technical and operational layers,
followed by an improvement plan and implementation.

**Timeline:** 3–4 weeks  
**Deliverables:**
1. Living document in this repo (updated throughout)
2. Technical assessment report (`05-deliverables/technical-report.md`)
3. Executive summary / presentation (`05-deliverables/executive-summary.md`)

---

## Assessment Domains

Every finding, analysis, and recommendation must reference one of these 12 domains:

| # | Domain | Short code |
|---|--------|-----------|
| 1 | Instrumentation | `INSTR` |
| 2 | Collection & Pipelines | `COLL` |
| 3 | Alerting | `ALERT` |
| 4 | Dashboards & Visibility | `DASH` |
| 5 | AKS Observability | `AKS` |
| 6 | Multi-Environment Consistency | `MULTI` |
| 7 | APM & Distributed Tracing | `APM` |
| 8 | Security Observability | `SEC` |
| 9 | Operational Processes | `OPS` |
| 10 | Governance | `GOV` |
| 11 | Observability Automation & CI/CD | `AUTO` |
| 12 | Datadog Platform Utilization | `DD` |

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
