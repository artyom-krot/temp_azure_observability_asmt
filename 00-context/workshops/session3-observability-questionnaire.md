# Session 3 — Observability / Datadog Deep Dive: Questionnaire

**Session:** Workshop 3 — Observability / Datadog Deep Dive
**Duration:** ~90 min
**Audience:** Datadog/Observability owner, SRE/Ops engineers

---

## Opening — Observability and Datadog

1. You've been running this Datadog environment for a couple of years now. What has been the biggest shift from where you started to where you are today?

   > **Notes:**

2. Which parts of the platform do you feel most confident monitoring right now — where you trust the data — and which parts do you feel less certain about?

   > **Notes:**

3. We've heard alert fatigue described as the top pain point. Before we dig in, can you give me a rough sense of the alert volume you're dealing with? Are we talking tens of alerts a day, hundreds, or something else?

   > **Notes:**

4. How do you deal with such an amount of alerts?

   > **Notes:**

---

## Area 1 — Observability Model and Responsibilities

1. Can you walk us through the current end-to-end observability architecture?

   > **Notes:**

2. Any systems remain monitored only through Azure-native tooling, or is everything in Datadog?

   > **Notes:**

3. What are the respective roles of Datadog, Azure Monitor, Log Analytics, and application-native logging?

   > **Notes:**

4. Who owns the Datadog platform, integrations, monitors, and telemetry onboarding?

   > **Notes:**

5. Is there a central observability team, or does each application team manage its own monitoring?

   > **Notes:**

6. How are clients, business landing zones, environments, and regions represented in Datadog?

   > **Notes:**

7. Can telemetry be reliably filtered by client, region, environment, application, and component?

   > **Notes:**

8. Which production environment should we use as the representative assessment scope?

   > **Notes:**

9. Does the selected environment include both legacy VM-based workloads and AKS workloads?

   > **Notes:**

10. Are primary and DR environments monitored in the same way?

    > **Notes:**

11. Are there known monitoring differences between regions or business landing zones?

    > **Notes:**

12. Who has admin access to Datadog, and what can a team member do to another team's monitors or dashboards?

    > **Notes:**

13. Is Datadog RBAC (Teams feature, or roles) configured — can you restrict a team to only see their own consumer's data?

    > **Notes:**

14. How do actual monitoring changes happen — what is the change process and how does it align with the application's SDLC?

    > **Notes:**

15. How is monitoring configuration deployed/released? CI/CD?

    > **Notes:**

---

## Area 2 — Telemetry Collection and Coverage

1. Which platform components currently send telemetry to Datadog?

   > **Notes:**

2. Are any known production components monitored only partially or not monitored at all?

   > **Notes:**

3. How is telemetry collected from each of the following tiers:
   - AKS clusters and workloads?
   - Virtual machines?
   - Application Gateway and WAF?
   - Service Bus?
   - Azure SQL Database?
   - Blob Storage?
   - Key Vault?
   - CDN and Apache?
   - Application services and microservices?

   > **Notes:**

4. Is there an inventory mapping critical components to their available logs, metrics, traces, health checks, and monitors?

   > **Notes:**

5. How do you verify that telemetry collection itself is healthy?

   > **Notes:**

6. Are there alerts for missing telemetry, disconnected agents, broken integrations, or stopped log pipelines?

   > **Notes:**

7. What are the most significant observability blind spots already known to the teams?

   > **Notes:**

8. Is there a known pain point with tracing today?

   > **Notes:**

9. Are there known trace gaps caused by legacy applications, asynchronous messaging, unsupported components, or sampling?

   > **Notes:**

---

## Area 3 — Logs

1. What application, infrastructure, platform, security, and audit logs are currently collected?

   > **Notes:**

2. How are logs sent to Datadog — agents, Azure integration, diagnostic settings, OpenTelemetry, direct APIs, or another mechanism?

   > **Notes:**

3. Are log formats and required fields standardized? Are application logs structured (JSON format / plaintext)?

   > **Notes:**

4. Can logs be consistently correlated using fields such as service, environment, version, client, region, trace ID, and request ID?

   > **Notes:**

5. Are there known parsing failures, missing fields, duplicate logs, or inconsistent service names?

   > **Notes:**

6. Are logs filtered or sampled before ingestion? If yes, could important operational events be excluded?

   > **Notes:**

7. Which logs remain only in Log Analytics, and why?

   > **Notes:**

8. Is the current one-month retention sufficient for incident investigation and SLA reporting?

   > **Notes:**

---

## Area 4 — Metrics and Health Signals

1. Which sources provide metrics — Azure platform metrics, Datadog Agent, Kubernetes integration, custom application metrics, or OpenTelemetry?

   > **Notes:**

2. Are standard resource health and availability signals collected for all critical Azure services?

   > **Notes:**

3. Do applications expose health, readiness, and liveness endpoints?

   > **Notes:**

4. Are application-level indicators available for availability, latency, throughput, saturation, and errors?

   > **Notes:**

5. Are asynchronous workloads monitored through signals such as queue depth, message age, dead-letter volume, processing failures, and consumer lag?

   > **Notes:**

6. Are there critical failure scenarios that cannot currently be detected using available metrics?

   > **Notes:**

7. Are custom metrics controlled to prevent excessive or unbounded cardinality?

   > **Notes:**

---

## Area 5 — Monitors and Alert Overload

1. Approximately how many active Datadog monitors exist?

   > **Notes:**

2. Can you walk us through the current alert inventory — how many active monitors exist in Datadog, and how are they organised?

   > **Notes:**

3. Are they organised by service, by team, by environment, or something else?

   > **Notes:**

4. How many alerts are generated per day or week, by severity and environment?

   > **Notes:**

5. Walk me through what a typical noisy week looks like. Which monitors fire most frequently, and which do you consider non-actionable?

   > **Notes:**

6. Are there monitors that are effectively permanently firing — that the team has mentally tuned out?

   > **Notes:**

7. Which monitors generate the highest alert volume?

   > **Notes:**

8. Which alerts are repetitive, duplicate, flapping, or commonly ignored?

   > **Notes:**

9. How are thresholds selected and reviewed?

   > **Notes:**

10. Are there monitors with thresholds set to values you know are wrong but haven't had time to fix?

    > **Notes:**

11. Are monitors primarily static-threshold, anomaly, forecast, composite, service-check, or log-based?

    > **Notes:**

12. Are warning and critical states operationally meaningful and handled differently?

    > **Notes:**

13. Do monitors include clear ownership, impact, affected scope, and response instructions?

    > **Notes:**

14. How are alerts grouped to avoid separate notifications for every pod, host, region, or client?

    > **Notes:**

15. Are dependencies or related symptoms correlated, suppressed, or deduplicated?

    > **Notes:**

16. How are maintenance, deployments, scaling events, and planned outages handled?

    > **Notes:**

17. Which alerts usually lead to real incidents, and which rarely require action?

    > **Notes:**

18. Are there known incidents that were detected late or were not detected at all?

    > **Notes:**

19. Can we obtain monitor definitions and alert-event history for quantitative analysis?

    > **Notes:**

20. When an alert fires, what actually happens — describe the full path from notification to someone taking action.

    > **Notes:**

21. Tell me about the database crash incident that caused the recent outage. When did monitoring first signal something was wrong — before or after consumers reported it? Was there a monitor watching that database — did it fire late, or did it not fire at all?

    > **Notes:**

---

## Area 6 — Integrations and Notification Routing

1. Which Datadog integrations are enabled?

   > **Notes:**

2. How is the Azure integration scoped across subscriptions, tenants, regions, and resource types?

   > **Notes:**

3. How are Datadog Agents deployed and upgraded across AKS and VMs?

   > **Notes:**

4. Which notification and incident integrations are used — email, Teams, PagerDuty, ServiceNow, bots, or custom webhooks?

   > **Notes:**

5. How is alert ownership and routing determined?

   > **Notes:**

6. Can alerts be routed using service, client, environment, region, severity, and support ownership?

   > **Notes:**

7. What happens when an alert has no identifiable owner?

   > **Notes:**

8. Are integration or notification delivery failures monitored?

   > **Notes:**

9. When a new consumer/environment is onboarded — how is observability set up for it?

   > **Notes:**

---

## Area 7 — Dashboards and Operational Usage

> *Lightweight — for discovery only.*

1. Which dashboards are actively used during normal operations and incidents?

   > **Notes:**

2. Is there a primary operational view showing platform health across critical environments?

   > **Notes:**

3. Who are the dashboard operators/consumers?

   > **Notes:**

4. Are dashboards generated from a common template or created independently?

   > **Notes:**

5. Are any critical systems effectively invisible unless someone searches for them manually?

   > **Notes:**

---

## Area 8 — Governance and Lifecycle

1. Are there documented standards for telemetry, tagging, monitor creation, naming, ownership, and severity?

   > **Notes:**

2. Which tags are mandatory, and how is compliance validated?

   > **Notes:**

3. Are there resources in Datadog today that are missing tags or have inconsistent tag values? How would you know?

   > **Notes:**

4. Is there a process that validates tag completeness before a resource goes into production monitoring?

   > **Notes:**

5. Who can create, modify, mute, or delete monitors?

   > **Notes:**

6. Is there an approval or peer-review process for production monitors?

   > **Notes:**

7. How are obsolete monitors, dashboards, integrations, and telemetry sources identified and removed?

   > **Notes:**

8. Are monitor and dashboard configurations managed manually or as code?

   > **Notes:**

9. How is a new application or service onboarded into observability?

   > **Notes:**

10. Is observability coverage validated before production release?

    > **Notes:**

11. Are ingestion volume, indexing, retention, and Datadog cost regularly reviewed?

    > **Notes:**

---

## Evidence to Request During or After the Workshop

- [ ] Current observability architecture or data-flow diagram
- [ ] Datadog service and infrastructure views
- [ ] Azure and Datadog integration configuration
- [ ] Active monitor inventory or export
- [ ] Alert-event history for a representative period
- [ ] Top noisy or recurring monitors
- [ ] Telemetry-volume breakdown by source, service, and environment
- [ ] Log pipelines, indexes, parsing rules, and retention configuration
- [ ] Tagging conventions
- [ ] Representative AKS, VM, Service Bus, SQL, and application telemetry
- [ ] Examples of missed incidents and noisy-alert incidents
- [ ] Existing monitoring standards and onboarding documentation
