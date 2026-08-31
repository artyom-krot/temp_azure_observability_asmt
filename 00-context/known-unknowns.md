# Known Unknowns — Pre-Assessment Blind Points

These are confirmed gaps in our knowledge before scope is finalised and discovery begins.
Each item represents something we cannot currently assess — it is either unmeasured, undocumented,
or not yet visible to the engagement team.

**Status:** Living document. Update status as each unknown is resolved during discovery.

---

## Blind Points

| # | Unknown | Domain(s) | How It Gets Resolved | Priority | Status |
|---|---|---|---|---|---|
| U-01 | **Monitoring coverage per tier** — which tiers (CDN, Apache, AKS, VMs, ASB, SQL, App Gateway, Key Vault) emit any signals at all; which are completely dark | COLL, INSTR | Architecture & Engineering Workshop (Session 2); Azure Monitor + Datadog config review | Critical | Open |
| U-02 | **Environment consistency** — whether all 12 production environments are configured the same way in Datadog and Azure Monitor, or whether configuration has drifted between regions | MULTI, COLL | Datadog API export across all envs; Azure diagnostic settings review per subscription | High | Open |
| U-03 | **Alert coverage vs. real failure modes** — which failure modes (DB crash, service unavailability, queue backlog, etc.) have a corresponding alert, and which are unmonitored | ALERT | Datadog monitor export + failure mode mapping; Operations Workshop (Session 4) | Critical | Open |
| U-04 | **SLA obligations mapped to observable signals** — whether contractual SLA commitments per consumer are backed by any SLO monitor, threshold, or automated signal | ALERT, OPS | Client-provided SLA/contract documentation; SLO export from Datadog; Session 4 | Critical | Open |
| U-05 | **APM instrumentation depth and trace coverage** — whether the Java application is instrumented with APM agents, which services emit traces, and whether distributed tracing is end-to-end or partial | APM, INSTR | Datadog APM service map; source code review; Application Observability Deep Dive (Session 5) | High | Open |
| U-06 | **Incident history and MTTD / MTTR baseline** — actual incident record beyond the known database crash: frequency, detection source (internal vs. consumer-reported), time-to-detect, time-to-resolve | OPS | Incident post-mortems; Operations Workshop (Session 4); support ticket history if available | High | Open |
| U-07 | **Operational strategy and governance state** — whether any monitoring standards, ownership rules, tagging policies, or on-call practices are formally defined or enforced | GOV, OPS | Governance & Operating Model Workshop (Session 6); any existing runbooks, wikis, or policy docs | High | Open |

---

## Notes

- **U-01 and U-03** are the highest-risk unknowns: they directly relate to the confirmed reactive detection problem.
- **U-04** has direct contractual exposure — if SLAs are breached silently, it is a business risk, not just a technical gap.
- **U-02** may be partially resolvable via Datadog API automation before workshops begin — plan to script this in Week 1.
- All items should be revisited after the Week 1 sessions; any still open by Week 2 Day 1 should be escalated as RAID risks.
