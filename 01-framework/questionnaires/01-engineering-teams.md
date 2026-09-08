# Workshop Questionnaire — Engineering Teams

**Audience:** Application developers, platform/infrastructure engineers  
**Duration:** 60–90 min  
**Goal:** Understand current instrumentation, tooling usage, and pain points at the implementation level

---

## Section 1 — Current State & Complaints

1. What are the most common complaints you receive about observability today?
2. When an incident occurs, what is your first action to investigate? Walk me through a recent example.
3. How do you typically find out about a production problem — alert, consumer complaint, or something else?
4. What monitoring or observability tool do you use most day-to-day? Why that one over others?
5. What is the one thing about your current observability setup that frustrates you most?

---

## Section 2 — Logging

6. Are application logs structured (JSON)? Which services, which are not?
7. Where do logs go — Log Analytics, Datadog, both, or somewhere else?
8. How do you search logs when investigating an incident? What tool, what query?
9. Do logs include a correlation or trace ID so you can follow a request across services?
10. Have you ever lost logs (gaps in log data)? Under what circumstances?
11. Is log verbosity something you can change at runtime, or does it require a redeploy?

---

## Section 3 — Metrics

12. What infrastructure metrics are you collecting today? (CPU, memory, disk, network)
13. Are application-level metrics emitted (request rate, latency, error rate)?
14. Are JVM metrics collected for Java services (heap, GC, thread counts)?
15. Are there any business-level metrics — e.g., transaction count per consumer, SLA tracking per environment?
16. How are custom metrics defined and emitted? Who is responsible for adding new ones?

---

## Section 4 — Tracing

17. Is distributed tracing implemented? Which tool — Datadog APM, OpenTelemetry, both?
18. Can you trace a request end-to-end from the entry point through all services to the database?
19. Do traces propagate across async boundaries — specifically Azure Service Bus messages?
20. What is the sampling rate? Is it configurable?
21. Have you ever been unable to debug a latency issue because trace data was missing or incomplete?

---

## Section 5 — Alerting

22. How many active alerts/monitors are there in Datadog? In Azure Monitor?
23. How often do you get paged? How many of those pages turn out to be non-actionable (false positives)?
24. Who owns alert configuration — is it the dev team, platform team, or shared?
25. When a threshold needs changing, what is the process? How long does it take?
26. Are all 12 environments (primary + DR per region) covered by the same alerts?
27. Are there any known blind spots — services or scenarios with no alerting?

---

## Section 6 — Dashboards

28. Which dashboards do you actually use regularly? Can you show me?
29. Who created your dashboards? Who maintains them?
30. Are there dashboards that are outdated or no longer accurate?
31. Is there a dashboard that shows the health of all 12 environments at once?
32. When a new service is deployed, who creates the dashboard for it?

---

## Section 7 — AKS-Specific

33. Is Container Insights enabled on the AKS clusters?
34. How do you monitor pod restarts and OOMKills?
35. Are HPA scaling events visible in your dashboards or alerts?
36. Is there visibility into namespace-level resource consumption?
37. Are AKS control plane logs (apiserver, scheduler) being collected?

---

## Section 8 — Observability & CI/CD

38. Are alert rules and dashboards stored in version control? If yes, where?
39. Is monitoring configuration deployed through your CI/CD pipeline?
40. When a new consumer environment is provisioned, how is observability set up — automated or manual?
41. Has monitoring configuration ever drifted between environments? How was it discovered?
