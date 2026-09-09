# Interview Guide — Session 2: Architecture & Engineering Workshop

**Date:** TBD (Week 1)
**Duration:** 120 minutes
**Audience:** Platform architects, Cloud/AKS engineers, Application tech leads
**Facilitated by:** Assessment team

**Session purpose:** Map the actual application and infrastructure landscape — what services exist, where they run, what flows between them, and what is business-critical. Secondary goal: agree on a reference environment (a single landing zone or environment subset) to anchor the observability deep-dive in Sessions 3 and 4.

**Out of scope for this session:** Monitoring tool configuration, alerting, Datadog specifics. Those are Session 3. If those topics come up, note them and park them.

---

## Pre-Session Checklist (for assessment team)

Before the session, confirm the following are in hand or explicitly noted as missing:
- Any architecture diagrams provided by the client (check `00-context/architecture/`)
- Infrastructure inventory or Terraform state export (requested at kick-off)
- In-scope Azure subscription IDs (requested at kick-off)
- Access to a non-production environment for later runtime inspection

Known from prior discovery that does NOT need re-asking:
- Infrastructure tier sequence (CDN to Key Vault) — broadly confirmed at kick-off
- Datadog as primary monitoring platform, single tenant, tag-based isolation — confirmed
- 12 production environments (primary + DR per region), US/UK/AU active, DE in-progress — confirmed
- Splunk-to-Datadog migration ~2 years ago — confirmed
- VM-to-AKS migration in progress — confirmed
- Core problem: reactive observability — confirmed

---

## Timing Guide

| Block | Topic | Suggested time |
|---|---|---|
| Rapport & framing | Opening, introductions, session goal | 10 min |
| A | Application and service topology | 25 min |
| B | Infrastructure and deployment model | 20 min |
| C | Data flows and dependencies | 15 min |
| D | Business criticality and SLA exposure | 15 min |
| E | Modernization state and trajectory | 10 min |
| F | Focus scope selection | 15 min |
| Close | Open floor / what keeps you up at night | 10 min |

**If time is short, protect blocks A, D, and F.** These are the highest-value blocks for the assessment. Blocks B and C can be partially covered by async evidence review if access has been provisioned.

---

## Opening / Rapport (10 min)

1. **Can you each give me a quick 60-second version of what you own — what layer of the platform are you closest to day-to-day?**

   > Listen for who owns AKS, application layer, messaging, legacy VMs. Note any gaps — if nobody introduces themselves as owning the messaging layer, database tier, or legacy VMs, that is already a signal.

2. **If I were a new engineer joining the team tomorrow and asked "what does this system actually do end-to-end?" — what would you tell me in plain language?**

   > Forces a service-level summary before going into topology. What they lead with is likely what they consider most critical.

3. **When did you last have to dig into how a specific part of this platform actually works — not how it should work, but how it actually behaves in production? What prompted that?**

   > Establishes how well they know their own system at runtime vs. design time. A vague answer is a signal about operational maturity.

---

## Block A — Application and Service Topology ★ (25 min)

**Highest priority block. Spend more time here if needed.**

**A1. Can you walk us through the services that make up this application — not the infrastructure they run on, but the actual services and what each one does?**

If they have a diagram, ask them to share screen and walk through it. If not, build it live.

Follow-up probes:
- "How many distinct services are there? Give me a rough count — even a range is fine."
- "Which of these are Java? Are any non-Java (Python, Node, .NET)?"
- "Are any of these still a monolith, or has everything been broken into microservices? If mixed, which parts are still monolithic?"
- "Are there any services that only some consumers use, or does every consumer environment run the full stack?"
- "Are there shared services that serve multiple consumers vs. per-consumer services?"

**A2. Which of these services are on VMs today, and which are on AKS? For those still on VMs — is there a plan to migrate them, or is that timeline undefined?**

Follow-up probes:
- "Can you give me a rough percentage — say, 30% VMs, 70% AKS?"
- "Are there services that are split — some instances on VMs, some on AKS?"
- "What is the blocker for the remaining VM services? Technical complexity, dependencies, resourcing?"

> The VM-to-AKS migration creates two fundamentally different instrumentation models running in parallel. Do not leave this block without a rough split percentage.

**A3. How are services deployed per consumer? Does each consumer get their own set of service pods/instances, or is there shared compute with logical isolation?**

Follow-up probes:
- "On AKS — is the pattern one namespace per consumer, one namespace per environment (primary/DR), or something else?"
- "Are there AKS clusters that serve multiple consumers, or is each cluster dedicated to one?"
- "Is the namespace model consistent across all regions, or did it evolve differently in US vs. UK vs. AU?"

> The namespace model directly determines how Datadog tag-based isolation is implemented, how RBAC scopes, and how alert routing must be structured. Get a concrete answer — draw it out if needed.

---

## Block B — Infrastructure and Deployment Model (20 min)

**B1. Starting at the edge: who is your CDN provider, and what does that tier do — caching, SSL offload, DDoS protection, or all of the above?**

Follow-up probes:
- "Is it Azure CDN, Cloudflare, Akamai, or something else?"
- "Does the CDN emit access logs? If so, do those go anywhere, or are they currently not being collected?"
- "Is WAF enforcement happening at the CDN layer, at the Application Gateway layer, or both?"

**B2. For the Apache layer — what is it actually doing here? Is it acting as a reverse proxy, handling SSL termination, doing URL routing, or something else?**

Follow-up probes:
- "Is Apache running on VMs or is it containerized?"
- "Is it present in every environment, or only certain regions?"
- "Are Apache access logs being captured anywhere?"

**B3. Walk me through how a new consumer environment is provisioned today. Is it scripted, manual, or a combination?**

Follow-up probes:
- "Is there a Terraform module or Helm chart that provisions a standard environment?"
- "How long does it take to stand up a new environment from scratch?"
- "Is observability setup — Datadog agents, Azure Monitor configuration — part of the provisioning automation, or done separately after the fact?"

> Whether environments are born consistent or manually configured is critical to assessing drift risk across 12 environments.

---

## Block C — Data Flows and Dependencies (15 min)

**C1. What are the primary data flows through the system — synchronous calls between services, and asynchronous flows through Service Bus?**

Follow-up probes:
- "Which flows are synchronous (HTTP/gRPC) vs. asynchronous (Service Bus messages)?"
- "How many Service Bus topics or queues are there, roughly?"
- "Are there any external integrations — third-party APIs, consumer systems that call in or receive callbacks?"
- "Is database access always through a single service, or do multiple services hit the DB directly?"

**C2. What does a real consumer request look like from their browser or API client all the way into the application?**

Follow-up probes:
- "Does traffic always go CDN → App Gateway → Apache → AKS, or are there paths that bypass some layers?"
- "Are there any API endpoints that consumers call directly, without going through the full stack?"
- "Are there background jobs or scheduled tasks that don't originate from consumer traffic at all?"

---

## Block D — Business Criticality and SLA Exposure ★ (15 min)

**Highest priority block. This determines the Tier 1 service list that anchors all alert coverage analysis.**

**D1. If this system had a partial failure right now — say one service goes down — which failure would cause the most immediate, visible impact to a consumer?**

Follow-up probes:
- "Would they get an error page, a degraded experience, or would the application silently stop doing something they depend on?"
- "Are there any failures that consumers would not notice immediately — things that degrade quietly over time?"
- "Is the impact the same for all consumers, or do different consumers rely on different parts of the system?"

**D2. Which services or flows have formal SLA commitments attached — things you are contractually obligated to keep available or performant?**

Follow-up probes:
- "Is there a shared understanding across the team of which services are Tier 1 (SLA-bound) vs. Tier 2 (best-effort)?"
- "Is that tier classification documented anywhere — a runbook, wiki, Confluence page? Or is it informal knowledge?"
- "Are there services you know are critical but don't currently have a formal SLA?"

**D3. When the database crashed recently and caused a consumer-facing outage — which service or flow failed first? Was it the database directly, or did something upstream fail and cascade?**

Follow-up probes:
- "Was this a single consumer's environment, or did multiple consumers see the impact?"
- "How did the team find out — internal alert, consumer call, or something else?"
- "Was there anything in the logs or metrics that, in hindsight, showed the problem building before the crash?"

> If the answer to "how did you find out" is "consumer called us" — capture this explicitly. It is direct MTTD evidence and one of the highest-severity findings available.

---

## Block E — Modernization State and Trajectory (10 min)

**E1. Where do you expect this platform to be in 12 months in terms of the VM-to-AKS migration? Is there a committed roadmap, or is it aspirational?**

Follow-up probes:
- "Are there specific services on the AKS roadmap in the next quarter?"
- "Are there services unlikely to ever move to AKS — too complex to containerize, or with external dependencies that prevent it?"
- "When DE goes live — is it being built on AKS only, or will it have the same VM/AKS mix as existing regions?"

> This shapes the target architecture. If VMs are temporary, investment in VM-specific observability should be minimal. If some VMs are permanent, they need first-class treatment.

**E2. Are there any significant architectural changes planned in the next 6 months — new services, new integrations, or external dependencies changing?**

Follow-up probes:
- "Anything that would change the data flows we just mapped?"
- "Any new consumer onboarding coming up that would stress the current provisioning model?"

---

## Block F — Focus Scope Selection ★ (15 min)

**Highest priority block. This is a structured decision — come in with a proposal based on blocks A–E, then validate it.**

**F1. We need to agree on a reference environment to anchor the observability deep-dive — one consumer/region combination that represents the "gold standard." Which environment would you nominate, and why?**

Guide toward selection using these criteria:
- Most representative of the full stack (all tiers present, both VM and AKS if applicable)
- Longest operational history (most mature configuration, most historical incidents)
- Not under active migration or restructuring right now
- Has an engaged stakeholder available during this assessment

Follow-up probes:
- "Is the US primary environment the most mature, or is one of the others further along?"
- "Is there an environment the team considers closest to best practice — one you'd point a new engineer at?"
- "Is DE too immature to use as a reference, or could it be interesting precisely because it is being built now?"

**F2. For the environments we don't focus on in depth — how consistent are they with the reference environment? Are they largely the same, or do you suspect meaningful drift?**

Follow-up probes:
- "Has anyone ever done a side-by-side comparison of monitoring configuration between regions?"
- "Are there any environments you'd flag as known outliers — configured differently for historical reasons?"

---

## Close — Open Floor (10 min)

**Close-1. Before we wrap — is there anything about how this system actually behaves in production that you think we would be surprised by? Something that doesn't show up in any diagram or document?**

> This is the highest-signal question of the session. Engineers who know their system will almost always have an answer. Listen for hidden dependencies, known dark spots, things they work around because they don't fully understand them.

**Close-2. What is the one thing about this platform that the assessment should absolutely not miss — the thing that would make this whole exercise worthwhile if we caught it?**

> Surfaces the most honest priority signal. Engineers will name what actually keeps them up at night, which is frequently different from what leadership stated at kick-off.

---

## Note-Taking Priorities

During the session, capture the following with particular care — these feed directly into the gap register:

| What to capture | Why it matters |
|---|---|
| All service names, their runtime (VM vs. AKS), and language | INSTR coverage baseline |
| Any service described as "not monitored," "we're not sure," or "a bit of a black box" | Immediate INSTR/COLL candidate finding |
| AKS namespace model — per consumer? per env? per region? | Drives tag governance and RBAC analysis in Session 3 |
| VM/AKS split percentage | Shapes instrumentation investment priorities |
| Services the team identifies as SLA-bound | Tier 1 service list for ALERT gap analysis |
| How the DB crash incident was detected — internal alert or consumer report | Direct MTTD evidence |
| Agreed reference environment (name/region) | Scopes Sessions 3, 4, and all subsequent analysis |
| Whether observability setup is part of provisioning automation | MULTI/AUTO finding signal |
| Services described as "in progress" for migration with no timeline | Assessment scope risk |

---

## Parking Lot

If these topics come up, acknowledge them and defer:

| Topic | Defer to |
|---|---|
| Datadog configuration, monitor counts, alert rules | Session 3 |
| On-call rotation, incident process, MTTD/MTTR history | Session 4 |
| Governance, ownership, access controls | Session 6 |
| APM, distributed tracing depth | Phase 2 (out of scope) |
