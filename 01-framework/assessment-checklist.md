# Observability Assessment Checklist

This checklist drives the gap analysis across all 12 domains.
For each item: mark `✅ Yes`, `❌ No`, `⚠️ Partial`, or `❓ Unknown`.
Unknown items become discovery actions (workshops or data pulls).

---

## D1 — Instrumentation (`INSTR`)

### Logging
- [ ] Application logs are structured (JSON or key-value), not plaintext
- [ ] Logs include correlation IDs / trace IDs for request tracking
- [ ] Log levels are used consistently (DEBUG/INFO/WARN/ERROR)
- [ ] Sensitive data (PII, secrets) is never logged
- [ ] All Java services log to a centralized sink (not local disk only)
- [ ] Log verbosity is configurable without redeployment

### Metrics
- [ ] Infrastructure metrics collected (CPU, memory, disk, network) for all VMs and AKS nodes
- [ ] Application-level metrics emitted (request rate, latency, error rate — RED pattern)
- [ ] JVM metrics collected (heap, GC, thread counts) for Java services
- [ ] Business-level metrics defined and collected (e.g., per-consumer transaction rates, SLA tracking)
- [ ] Custom metrics emitted for domain-specific KPIs

### Traces
- [ ] Distributed tracing is implemented across service boundaries
- [ ] OpenTelemetry SDK is used (or Datadog APM agent with auto-instrumentation)
- [ ] Trace context propagates across async boundaries (Azure Service Bus messages)
- [ ] Database calls are traced (Azure SQL)
- [ ] External HTTP calls are traced
- [ ] Sampling strategy is defined and documented

---

## D2 — Collection & Pipelines (`COLL`)

- [ ] All AKS pods emit logs to a centralized Log Analytics Workspace
- [ ] All VMs have the Azure Monitor Agent (AMA) or Log Analytics agent installed
- [ ] Datadog Agent is deployed on all relevant hosts/pods
- [ ] Kubernetes events are collected
- [ ] Azure resource diagnostic logs are forwarded (App Gateway, ASB, SQL, Key Vault, AKS)
- [ ] Log ingestion pipeline has no single point of failure
- [ ] Data pipeline latency is monitored (time from emit to query-available)
- [ ] Log retention periods are defined and match compliance requirements
- [ ] Log Analytics workspace per region or centralized? (document the topology)

---

## D3 — Alerting (`ALERT`)

- [ ] Alerts exist for all Tier 1 services (defined list)
- [ ] Alerts are symptom-based (user-visible impact), not just cause-based (CPU spike)
- [ ] SLO/SLA breach alerts are defined for each consumer environment
- [ ] Alert thresholds are based on baselines, not arbitrary values
- [ ] Alert routing is configured (right alert → right team)
- [ ] Alert ownership is documented per service
- [ ] Escalation policy is defined (P1/P2/P3 classification)
- [ ] False positive rate is measured and tracked
- [ ] Alert tuning process exists (regular review cadence)
- [ ] Alerts cover all 12 environments (not just primary or a subset)
- [ ] Flapping/noisy alerts are suppressed with appropriate conditions
- [ ] Dead man's switch / heartbeat alerts exist for critical pipelines

---

## D4 — Dashboards & Visibility (`DASH`)

- [ ] Executive dashboard exists (SLA/availability per consumer, trend view)
- [ ] Operational dashboards exist per service (RED metrics, error rates, saturation)
- [ ] Infrastructure dashboards exist (AKS node health, VM utilization, ASB queue depth)
- [ ] Per-consumer dashboards available (multi-tenant visibility per environment)
- [ ] Capacity dashboards exist (growth trends, approaching limits)
- [ ] Dashboards are reviewed and kept up to date (not stale)
- [ ] Dashboard ownership is assigned
- [ ] Dashboards use consistent time ranges and variables
- [ ] Runbook links are embedded in dashboards and alert notifications

---

## D5 — AKS Observability (`AKS`)

- [ ] Container Insights is enabled on all AKS clusters
- [ ] Node-level metrics collected (CPU, memory, disk pressure)
- [ ] Pod-level metrics collected per namespace
- [ ] kube-state-metrics deployed and scraped
- [ ] HPA (Horizontal Pod Autoscaler) events monitored
- [ ] OOMKill events alerted
- [ ] PersistentVolume usage monitored
- [ ] AKS control plane logs forwarded (apiserver, scheduler, controller-manager)
- [ ] Namespace-level resource quotas monitored
- [ ] Network policies violations captured
- [ ] Cluster upgrade events tracked

---

## D6 — Multi-Environment Consistency (`MULTI`)

- [ ] All 12 environments (primary + DR) have identical monitoring configuration
- [ ] Monitoring config is defined as code (not manually configured per env)
- [ ] DR environments are actively monitored (not dormant until failover)
- [ ] Per-region dashboards exist
- [ ] Cross-region comparison is possible (same metrics, same dashboards)
- [ ] New environment provisioning includes automated observability setup
- [ ] Configuration drift detection exists between environments
- [ ] Tagging strategy is consistent across all environments (env, region, consumer, service)

---

## D7 — APM & Distributed Tracing (`APM`)

- [ ] Datadog APM is enabled and receiving traces
- [ ] Service map / dependency map is available and accurate
- [ ] End-to-end traces visible from frontend → backend → database
- [ ] Trace-to-log correlation is working (trace ID in logs)
- [ ] P50/P95/P99 latency tracked per endpoint
- [ ] Error tracking configured (Datadog Error Tracking or equivalent)
- [ ] Profiling enabled for performance-critical Java services
- [ ] Deployment tracking enabled (correlate deploys with performance changes)
- [ ] Async message flows traceable (ASB message tracing)

---

## D8 — Security Observability (`SEC`)

- [ ] Azure Application Gateway / WAF logs are collected and analyzed
- [ ] WAF rule violations alerted
- [ ] Azure Key Vault access logs forwarded and monitored
- [ ] Unusual access patterns to Key Vault alerted (credential stuffing, unexpected callers)
- [ ] Azure AD / Entra ID sign-in logs collected
- [ ] Privileged identity actions logged and alerted
- [ ] AKS audit logs enabled and forwarded
- [ ] Network flow logs (NSG flow logs) enabled
- [ ] Datadog Cloud SIEM configured (if licensed)
- [ ] Security incident alerting has defined escalation path

---

## D9 — Operational Processes (`OPS`)

- [ ] On-call rotation is defined and documented
- [ ] Incident severity classification (P1/P2/P3/P4) is defined
- [ ] Incident management tooling is in place (PagerDuty / ServiceNow / Teams)
- [ ] Runbooks exist for all Tier 1 alerts
- [ ] Runbooks are linked from alert notifications
- [ ] Post-incident review (PIR/RCA) process is defined
- [ ] MTTD (Mean Time to Detect) is measured
- [ ] MTTR (Mean Time to Recover) is measured
- [ ] SLA reporting to consumers is automated
- [ ] Scheduled maintenance communication process exists
- [ ] War room / incident bridge process defined for P1

---

## D10 — Governance (`GOV`)

- [ ] Observability ownership model defined (who owns each domain/service)
- [ ] Tagging strategy defined and enforced (environment, region, consumer, service, team)
- [ ] Observability cost is tracked and budgeted (Log Analytics ingestion, Datadog usage)
- [ ] Log ingestion costs reviewed regularly
- [ ] Unused dashboards and alerts pruned on a schedule
- [ ] Observability standards documented and communicated to all teams
- [ ] New service onboarding checklist includes observability requirements
- [ ] Teams are trained on observability tooling

---

## D11 — Observability Automation & CI/CD (`AUTO`)

- [ ] Alert rules are version-controlled (Terraform / Bicep / Datadog Terraform provider)
- [ ] Dashboards are version-controlled (Datadog dashboard JSON in repo)
- [ ] Monitor/alert deployment is part of CI/CD pipeline
- [ ] New consumer environment provisioning automates observability setup (no manual steps)
- [ ] Base observability template exists that all environments inherit
- [ ] Synthetic monitoring tests are automated and part of release pipeline
- [ ] Observability config changes require code review (same as application code)
- [ ] Drift detection between declared and actual monitoring config is automated
- [ ] Chaos / fault injection testing validates alert coverage

---

## D12 — Datadog Platform Utilization (`DD`)

### Module Coverage (Licensed vs. Active)
- [ ] Infrastructure Monitoring — agents deployed, dashboards active
- [ ] APM — traces received, service map populated
- [ ] Log Management — logs ingested, pipelines configured
- [ ] Synthetic Monitoring — synthetic tests defined for critical user journeys
- [ ] Database Monitoring (DBM) — Azure SQL monitored via Datadog DBM
- [ ] Network Performance Monitoring — enabled if licensed
- [ ] Cloud Security Management / Cloud SIEM — enabled if licensed
- [ ] Real User Monitoring (RUM) — if there is a web frontend

### Datadog Configuration Quality
- [ ] Datadog Agent version is current (within 1 major version of latest)
- [ ] Unified Service Tagging applied (`env`, `service`, `version` tags on all resources)
- [ ] Datadog API keys scoped correctly (not a single global key for everything)
- [ ] Datadog monitors have defined notification channels (not just email)
- [ ] Datadog SLOs defined for critical services
- [ ] Datadog Notebooks used for incident investigation (or equivalent)
