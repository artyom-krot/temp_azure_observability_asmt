# Interview Guide — Session 3: Observability / Datadog Deep Dive

**Session:** Workshop 3 — Observability / Datadog Deep Dive
**Date:** To be scheduled (Week 1)
**Duration:** 90 minutes
**Audience:** Datadog/Observability owner, SRE/Ops engineers
**Facilitated by:** Assessment team
**Style:** Engineering-calibrated — go deep on configurations, actual numbers, and what is real vs. what should be

---

## What We Already Know (Do Not Re-Ask)

- Datadog is the primary monitoring platform; single tenant for all consumer environments
- Migration from Splunk completed approximately 2 years ago
- Alert fatigue is an explicit, named pain point — confirmed by the client at kickoff
- No Datadog RBAC/governance guardrails — client raised this themselves as a concern
- SLA/SLO monitoring confidence is low — client explicitly asked whether it exists
- A recent database crash caused a consumer-facing outage — detection was reactive

---

## Timing Guide

| Block | Topic | Suggested time |
|---|---|---|
| Opening | Rapport and orientation | 5 min |
| 1 | Alert inventory and fatigue ★ | 25 min |
| 2 | Single-tenant governance and tag model ★ | 15 min |
| 3 | Agent deployment and collection coverage | 15 min |
| 4 | Log pipeline and structure | 10 min |
| 5 | SLO configuration | 10 min |
| 6 | Metrics and custom instrumentation | 8 min |
| 7 | APM current state (Phase 2 scoping) | 5 min |
| Close | Open floor | 2 min |

**If time runs short, protect Themes 1 and 2.** Themes 3–6 are partially recoverable via artifact review after the session.

---

## Opening — Rapport and Orientation (5 min)

1. **Before we get into specifics — you've been running this Datadog environment for a couple of years now. What has been the biggest shift from where you started to where you are today?**

2. **Which parts of the platform do you feel most confident monitoring right now — where you trust the data — and which parts do you feel less certain about?**

3. **We've heard alert fatigue described as the top pain point. Before we dig in, can you give me a rough sense of the alert volume you're dealing with? Are we talking tens of alerts a day, hundreds, or something else?**

   > The third question sets a concrete anchor for the entire alerting section. Listen for whether they express relief, frustration, or resignation — all are signals about operational culture.

---

## Theme 1 — Alert Inventory and Fatigue ★ (25 min)

**Priority 1 — #1 client pain point. Spend more time here if needed.**

**Q1. Can you walk us through the current alert inventory — how many active monitors exist in Datadog, and how are they organised?**

Follow-up probes:
- "Are they organised by service, by team, by environment, or something else?"
- "Are monitors duplicated per environment — so the same logic exists 12 times — or are they shared with multi-alert configuration?"
- "Is there any way to pull a count quickly, or would you need to export it? We'd love to see the raw export if possible."
- "Are there also active alerts in Azure Monitor running in parallel to Datadog? If so, roughly how many?"

> Listen for whether monitors are per-environment (strong indicator of configuration debt and volume source), and whether Azure Monitor is doubling the alert surface.

---

**Q2. Walk me through what a typical noisy week looks like. Which monitors fire most frequently, and which do you consider non-actionable?**

Follow-up probes:
- "Are there monitors that are effectively permanently firing — that the team has mentally tuned out?"
- "Are any monitors currently muted or silenced? Why were they silenced, and how long have they been that way?"
- "What percentage of pages you receive in a week are ones someone actually acts on versus ignores or closes immediately?"
- "Are there monitors with thresholds set to values you know are wrong but haven't had time to fix?"

> Muted monitors are a critical finding — they represent blind spots created to cope with noise. "We just know to ignore that one" is a direct finding.

---

**Q3. When an alert fires, what actually happens — describe the full path from notification to someone taking action.**

Follow-up probes:
- "Where does the notification go — PagerDuty, OpsGenie, Slack, email, Teams, or a mix?"
- "Is routing differentiated by severity? Does a P1 wake someone up at 3am, or does everything go to the same channel?"
- "Do monitors link to a runbook? If so, where are runbooks stored?"
- "If the alert fires at 2am and the on-call person doesn't know what it means, what do they do?"

> No severity-based routing, no runbook links, and single-channel notifications are all findings. The last probe surfaces whether on-call engineers are equipped to act.

---

**Q4. Tell me about the database crash incident that caused the recent outage. When did monitoring first signal something was wrong — before or after consumers reported it?**

Follow-up probes:
- "Roughly how long passed between the database failing and the team being notified?"
- "Was there a monitor watching that database — did it fire late, or did it not fire at all?"
- "What was the trigger that actually started the incident response — monitoring alert, consumer complaint, or something else?"
- "Looking back, was there a signal in the data that a monitor could have caught earlier?"

> If monitoring did not fire at all, that is a coverage gap. If the consumer called first, that confirms reactive detection with a specific incident anchor. Record MTTD if they can estimate it.

---

**Q5. Are there known blind spots — services, tiers, or failure modes where you know monitoring doesn't cover you?**

Follow-up probes:
- "Are the DR environments (secondary per region) monitored to the same level as primary, or lighter?"
- "Is Azure Service Bus monitored — queue depth, message age, dead-letter queue depth?"
- "Is the CDN layer monitored? Do you get alerting on CDN-level errors, or only once it reaches Apache?"
- "What about Azure Key Vault — any alerting on certificate expiry or unexpected secret access?"

> DR environments being unmonitored is a common gap. Azure PaaS services (Service Bus, Key Vault, SQL) are often monitored at infrastructure level but not at application-behaviour level.

---

## Theme 2 — Single-Tenant Governance and Tag Model ★ (15 min)

**Priority 2 — Highest architectural risk in this stack. Tag-based isolation is the only separation mechanism between consumer environments.**

**Q6. Walk me through the tagging model. How are resources tagged in Datadog, and what enforces that tagging?**

Follow-up probes:
- "Are `env`, `consumer`, and `service` tags applied consistently to all agents, all log pipelines, all monitors?"
- "What happens if a new service is deployed and the developer forgets to apply the `consumer` tag — how is that caught?"
- "Is there a process that validates tag completeness before a resource goes into production monitoring?"
- "Are there resources in Datadog today that are missing tags or have inconsistent tag values? How would you know?"

> If the answer to "how is it enforced" is "we trust teams to do it," that is a governance gap. No automated tag validation is a finding.

---

**Q7. Who has admin access to Datadog, and what can a team member do to another team's monitors or dashboards?**

Follow-up probes:
- "Is Datadog RBAC (Teams feature, or roles) configured — can you restrict a team to only see their own consumer's data?"
- "If I'm an engineer on the UK consumer team, can I accidentally modify a monitor that applies to the US consumer?"
- "Have you ever had an incident where a monitor was changed or deleted by someone who shouldn't have touched it?"
- "Are API keys scoped to a specific team or service, or is there a single global API key in use?"

> Single global API keys and no role-based restrictions are findings. In a single-tenant model, absence of RBAC is a material risk to data isolation and operational stability.

---

**Q8. When a new consumer environment is onboarded — like DE, which is in progress — how is observability set up for it?**

Follow-up probes:
- "Is there a checklist, a template, or is it ad-hoc?"
- "How long does it take to reach the same monitoring coverage as an existing production environment?"
- "Are monitors created manually in the Datadog UI, or is there any infrastructure-as-code (Terraform, Pulumi) involved?"
- "For DE — does monitoring for that environment already exist, even partially?"

> Manual, ad-hoc onboarding is a finding in both MULTI and GOV domains. If monitors are UI-created and manually replicated, configuration drift between environments is nearly certain.

---

## Theme 3 — Agent Deployment and Collection Coverage (15 min)

**Q9. How is the Datadog agent deployed on AKS — and is it deployed the same way across all clusters?**

Follow-up probes:
- "Is it deployed as a DaemonSet via the Datadog Helm chart, or another method?"
- "Which Datadog agent version is running — is it kept current?"
- "Are all namespaces covered, or are there namespaces where the agent doesn't collect?"
- "Are Kubernetes events (pod restarts, OOMKills, scheduling failures) being collected?"
- "Is the Cluster Agent deployed alongside the node agents?"

> A DaemonSet deployed via Helm is the correct approach — anything else is a finding. Namespace exclusions create blind spots.

---

**Q10. How is the Datadog agent deployed on Azure VMs — and are all VMs covered?**

Follow-up probes:
- "Are agents deployed via cloud-init, Ansible, a VM extension, or something else?"
- "Is there a way to confirm that every production VM has an agent running — or could there be VMs that were missed?"
- "Do VMs have the Azure Monitor Agent (AMA) installed alongside Datadog, or is it one or the other?"

> Manual agent installation on VMs almost always means missed coverage. If both AMA and Datadog agents are installed, there may be double-ingestion and cost implications.

---

**Q11. Is the Azure Monitor / Azure integration configured in Datadog — are Azure resource metrics flowing in?**

Follow-up probes:
- "Is the Datadog Azure integration tile configured — are you pulling Azure resource metrics via integration, or only via the agent?"
- "Are diagnostic settings enabled on Azure SQL, Service Bus, and App Gateway instances?"
- "If I wanted to see Azure SQL DTU utilisation or Service Bus dead-letter queue depth in a Datadog dashboard, could I — or would I need to go to Azure Monitor?"
- "Are Azure Monitor alerts running in parallel to Datadog monitors for any of these resources?"

> If Azure PaaS metrics only exist in Azure Monitor and not in Datadog, the team is context-switching between two tools during incidents. Double-alerting (both Azure Monitor and Datadog firing for the same condition) inflates alert volume.

---

## Theme 4 — Log Pipeline and Structure (10 min)

**Q12. Are Java application logs structured — JSON format — or are they plaintext?**

Follow-up probes:
- "Is it consistent across all services, or are some structured and others not?"
- "Do logs include a trace ID or correlation ID that links a log line to a specific request or Datadog trace?"
- "Are there Datadog log pipelines configured to parse, enrich, or transform logs — or do raw lines land without processing?"
- "Are any logs sampled or filtered before ingestion — for example, dropping DEBUG logs in production to control cost?"

> Unstructured logs mean free-text search only — no reliable alerting on log-based monitors, slow incident investigation. Missing trace/correlation IDs breaks log-to-trace correlation.

---

**Q13. The 30-day retention figure was mentioned at kickoff. Is there a log archive configured beyond that?**

Follow-up probes:
- "If we need logs from 60 or 90 days ago for an incident investigation or compliance request, is that possible?"
- "Are all log indexes subject to the same 30-day retention, or do specific indexes have different policies?"
- "Is log ingestion volume monitored — do you know how much you're ingesting per day?"

> No archive beyond 30 days is a confirmed gap. For DE and CA, regulatory environments may elevate this to Critical.

---

## Theme 5 — SLO Configuration (10 min)

**Q14. Are Datadog SLOs configured — and if so, what do they cover?**

Follow-up probes:
- "Can you show me the SLO list in Datadog? How many are defined?"
- "Are SLOs tied to contractual SLA commitments — for example, a 99.9% uptime SLA with a specific consumer — or are they informally defined targets?"
- "Are there SLO alert monitors — alerts that fire when you are on track to breach an SLO or have already breached it?"
- "Are SLO dashboards shared with consumers or leadership, or are they internal-only?"
- "Are error budgets tracked and acted on?"

> If no SLOs exist at all, that is a Critical finding given enterprise SLA commitments across 12 environments. If SLOs exist but aren't linked to contractual SLAs, that is a gap between operational visibility and business accountability.

---

## Theme 6 — Metrics and Custom Instrumentation (8 min)

**Q15. What application-level metrics are the Java services emitting — are you collecting request rates, latency, and error rates per service?**

Follow-up probes:
- "Are these emitted via the Datadog agent's JMX integration, via Micrometer/StatsD, or via the Datadog APM agent's auto-instrumentation?"
- "Are JVM metrics collected — heap usage, garbage collection duration, thread counts?"
- "Are there any custom business metrics — for example, transactions processed per consumer per hour, or queue processing throughput?"
- "Is there any per-consumer or per-environment breakdown in your metrics, or are metrics aggregated across all consumers?"

> If the only metrics are infrastructure-level (CPU, memory, disk), there is no application-layer visibility. Absence of per-consumer metric dimensions means the team cannot tell which consumer is affected during an incident without digging through logs.

---

## Theme 7 — APM Current State (Phase 2 Scoping — 5 min)

**APM is deferred to Phase 2. Goal here is only to establish a baseline.**

**Q16. Is Datadog APM deployed on the Java services — are you receiving traces today?**

Follow-up probes:
- "Is it the Datadog Java agent (dd-java-agent.jar) or OpenTelemetry with a Datadog exporter?"
- "How many services are visible in the Datadog service map — is it a handful of core services, or most of the application?"
- "Is APM active in all 12 environments or only a subset?"
- "Is there a known pain point with tracing today that we should note for Phase 2?"

> You need a yes/no on whether APM exists and a rough coverage estimate. If APM is licensed but not deployed, that is a DD domain finding.

---

## Close — Open Floor (2 min)

**Q17. Final question — and feel free to be blunt. What is the one thing about your current observability setup that you lie awake worrying about? Something that isn't on our list, or something you think we might be underestimating.**

Secondary prompt if they go quiet: *"Alternatively — if something went seriously wrong tonight, what is the failure mode you're least confident you would detect in time?"*

---

## Note-Taking Priorities

| What to capture | Why it matters | Domain |
|---|---|---|
| Total monitor count and how they're organised | Alert inventory baseline | ALERT |
| Names/descriptions of muted or permanently-firing monitors | Alert fatigue evidence — direct findings | ALERT |
| How the DB crash was detected — alert or consumer report | MTTD evidence with specific incident anchor | ALERT |
| Notification routing path (PagerDuty? Slack? severity-split?) | Alert routing gap assessment | ALERT, OPS |
| Tag enforcement mechanism — automated or trust-based | GOV tag governance finding | GOV |
| RBAC / Teams configuration state | Single-tenant isolation risk | GOV |
| How new environments are onboarded for observability | MULTI consistency risk | MULTI, GOV |
| AKS agent deployment method and namespace coverage | COLL coverage baseline | COLL |
| VM agent deployment method and coverage confidence | COLL coverage baseline | COLL |
| Azure Monitor integration status in Datadog | Context-switching and double-alerting risk | COLL, ALERT |
| Whether Java logs are structured and have trace IDs | INSTR log quality | INSTR |
| Log retention beyond 30 days — archive configured? | COLL retention gap | COLL |
| SLO count and whether linked to contractual SLAs | SLA/SLO coverage gap | INSTR, ALERT |
| Custom business metrics — per-consumer breakdown? | INSTR application visibility | INSTR |
| APM deployed yes/no, coverage estimate | Phase 2 baseline | APM |

---

## Artifacts to Request During or After This Session

| Artifact | Purpose | Domain |
|---|---|---|
| Datadog monitor export (full list or screenshot of count by status) | Alert inventory baseline | ALERT |
| List of muted/silenced monitors with duration | Alert fatigue evidence | ALERT |
| Datadog Teams / RBAC configuration screenshot | Governance evidence | GOV |
| Tagging policy documentation (if it exists) | Tag enforcement evidence | GOV |
| Datadog SLO list (screenshot or export) | SLO coverage baseline | INSTR, ALERT |
| Datadog agent Helm values file (AKS) | Agent deployment evidence | COLL |
| Log index configuration and retention settings | Retention gap evidence | COLL |
| Log pipeline configuration (parsing rules) | Log quality evidence | INSTR |
| Licensed Datadog modules list | Platform utilisation | DD |
| Azure Monitor alert rules (if any active) | Double-alerting check | ALERT |

---

## Red Flags — Escalate Finding Severity if You Hear These

| What you hear | Finding signal |
|---|---|
| "We have monitors but I'm not sure what they all cover" | Absent ownership — GOV finding |
| "We muted that one because it was always firing" | Blind spot created by fatigue — Critical ALERT finding |
| "Consumers called us before we knew" (DB incident) | Confirmed reactive detection — Critical ALERT finding |
| "There's just one person who understands Datadog" | Single point of failure — OPS/GOV finding |
| "The tags are supposed to be there but I'm not 100% sure" | Tag governance failure — GOV finding |
| "Azure Monitor also fires on that" | Double-alerting contributing to volume — ALERT finding |
| "The DR environments have less coverage" | Multi-environment consistency gap — MULTI finding |

---

## Parking Lot

If these topics come up, acknowledge and defer:

| Topic | Defer to |
|---|---|
| Application/service topology, namespace model | Session 2 (or note if already captured) |
| On-call rotation, escalation paths, runbook process depth | Session 4 |
| Governance ownership, standards adoption, access lifecycle | Session 6 |
| APM architecture, distributed tracing design | Phase 2 |
