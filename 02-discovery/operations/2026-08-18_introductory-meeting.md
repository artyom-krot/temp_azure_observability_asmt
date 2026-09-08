# Discovery: Introductory Client Meeting

**Date:** 2026-08-18
**Source:** Introductory meeting — verbal, unstructured
**Domains touched:** ALERT, DD, GOV, APM, INSTR, COLL, MULTI, OPS

## Evidence Summary

First meeting with the client. Covers their current monitoring stack, key pain points, concerns, and the ask for this engagement. Several important architectural facts clarified that differ from initial assumptions.

---

## Observations

### DD — Datadog Migration and Stack
**Evidence:** Client migrated from Splunk to Datadog approximately 2 years ago. Single Datadog tenant used for all consumer environments (not per-consumer tenants).
**Checklist item:** Datadog platform utilization — tenant architecture
**Assessment signal:** Needs clarification ❓
**Notes:** Single-tenant model with multi-consumer data is a critical architectural fact. Tag-based isolation (env, consumer) is the only separation mechanism. This makes UST and tag governance a high-priority area. [SECURITY]

### COLL — Log Sources
**Evidence:** "CDN, Apache, Microservices, DB — all log to Datadog"
**Checklist item:** Log collection coverage — all tiers present
**Assessment signal:** Partial ⚠️
**Notes:** Log ingestion appears broad but depth of pipeline configuration (parsing, enrichment, indexing strategy) is unknown. Azure Monitor collection is not mentioned — unclear if platform-level Azure signals flow to Datadog or remain only in Azure Monitor.

### COLL — Log Retention
**Evidence:** "1 month retention"
**Checklist item:** Log retention policy — minimum 90 days hot for production
**Assessment signal:** Gap ❌
**Notes:** 30-day online index retention is below the 90-day minimum for effective incident investigation and trend analysis. Archive configuration (to cold storage) is unknown — if no archive exists, logs older than 30 days are permanently lost.

### ALERT — Primary Pain Point
**Evidence:** Client explicitly called out alerting as a primary pain point.
**Checklist item:** Alert quality — actionable, routed, low false-positive rate
**Assessment signal:** Gap ❌
**Notes:** No specifics provided yet. Follow up in engineering workshop: what makes current alerting painful? Volume? False positives? Missing coverage? Routing problems?

### APM / OPS — Recent Critical Incident
**Evidence:** "Recent database crash leading to outage" cited as example pain point.
**Checklist item:** MTTD for Tier 1 services; database monitoring; incident detection
**Assessment signal:** Gap ❌
**Notes:** A database crash resulting in a customer outage strongly implies a detection gap — monitoring did not catch the DB failure proactively. This is the concrete example of reactive observability. Must determine: was this detected by monitoring or by consumers? How long between failure and detection?

### GOV — Guardrails Concern
**Evidence:** Client raised concern: "No guardrails? Customers can do whatever they want" — governance question about who can modify monitors/dashboards in the single Datadog tenant.
**Checklist item:** Datadog RBAC, monitor ownership, governance controls
**Assessment signal:** Gap ❌
**Notes:** With a single Datadog tenant and multiple teams, absence of RBAC and monitor ownership policies means any team can create, modify, or delete monitors affecting all consumers. This is both a governance and reliability risk. [SECURITY]

### DD — Module Utilization
**Evidence:** "Are we using all Datadog capabilities?" — explicit client question.
**Checklist item:** Licensed vs. active modules
**Assessment signal:** Needs clarification ❓
**Notes:** Client is not confident they are using their Datadog investment fully. SLO monitoring is specifically called out as uncertain. Must map licensed modules vs. configured vs. actively used.

### INSTR / ALERT — SLA/SLO Monitoring
**Evidence:** "Do we monitor SLA/SLO?" — explicit client question. SLA/SLO definitions will be shared later.
**Checklist item:** SLO configuration in Datadog; business metrics instrumentation
**Assessment signal:** Gap ❌
**Notes:** Client does not have confidence that SLAs/SLOs are being monitored. SLO definitions will be provided — once received, map them to existing Datadog SLO monitors (or confirm none exist). This is a High priority given reliability/stability are stated key drivers.

### OPS — Principles and Guidelines Ask
**Evidence:** "Need to leave the team with principles/guidelines"
**Checklist item:** Observability governance documentation; runbook standards
**Assessment signal:** Gap ❌
**Notes:** Client explicitly wants the engagement to produce guidelines they can follow going forward — not just a point-in-time assessment. Deliverables should include governance principles document alongside the roadmap.

---

## Checklist Items Confirmed ✅

- Datadog is the primary monitoring platform (confirmed)
- All application tiers (CDN, web, microservices, DB) have some log coverage in Datadog
- Bots and alerts exist (some automation in place — depth unknown)

## Checklist Items Flagged as Gaps ❌

- Log retention: 30 days confirmed, below 90-day minimum
- Alerting quality: explicitly called out as a pain point
- SLA/SLO monitoring: client uncertain this exists
- Governance/RBAC in Datadog: no guardrails confirmed as concern
- Proactive detection: DB crash → outage confirms reactive posture

## Open Questions for Workshops

1. DB crash incident: Was it detected by monitoring or by consumers? What was MTTD?
2. What specifically makes alerting painful — volume, noise, routing, missing alerts?
3. What Datadog modules are currently licensed? (need license details)
4. Is there a log archive configured beyond the 30-day online index?
5. How are consumers/environments isolated within the single Datadog tenant? (tag strategy)
6. What does the on-call rotation look like? PagerDuty/OpsGenie integration?
7. Are there existing runbooks for any alerts?
8. What bots and automation are in place?
9. CDN provider — is it Azure CDN, Cloudflare, or other? What logs does it emit?
10. SLA/SLO definitions — awaited from client.
