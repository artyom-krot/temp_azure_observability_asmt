# Session 2 — Architecture & Engineering Workshop: Questionnaire

**Session:** Workshop 2 — Architecture & Engineering Workshop
**Duration:** ~120 min
**Audience:** Platform architects, Cloud/AKS engineers, Application tech leads

---

## Area 1 — HighQ Platform Purpose and Boundaries

1. What is the platform size — can we measure it somehow (clients, regions, BLZ, delivery teams)?

   > **Notes:**

2. What business capabilities does the platform provide?

   > **Notes:**

3. Who are its main users or consumers/clients?

   > **Notes:**

4. What are the main applications, products, or business services within the platform?

   > **Notes:**

5. Where does responsibility for this platform begin and end?

   > **Notes:**

6. Which external systems, customer systems, or shared enterprise services are outside the platform boundary but critical to its operation?

   > **Notes:**

7. Are there existing architecture diagrams, service catalogs, CMDB records, or deployment inventories that accurately represent the current state?

   > **Notes:**

8. Are there any significant architectural changes planned in the next 6–12 months — new services, new integrations, or external dependencies changing?

   > **Notes:**

---

## Area 2 — Criticality and Business Impact

1. Which applications or services are considered business-critical?

   > **Notes:**

2. Is business-criticality different for some clients/BLZ?

   > **Notes:**

3. What failures would have the greatest customer or business impact?

   > **Notes:**

4. Which services or flows have formal SLA commitments with clients — things you are contractually obligated to keep available or performant?

   > **Notes:**

5. Are there formal criticality tiers for applications or infrastructure components?

   > **Notes:**

6. Which components have explicit availability, performance, recovery, or regulatory requirements?

   > **Notes:**

7. Are there known architectural bottlenecks, fragile components, or single points of failure?

   > **Notes:**

8. When the database crashed recently and caused an outage — which service or flow failed first? Was it the database directly, or did something upstream fail and cascade? Was this a single consumer's environment, or did multiple consumers see the impact?

   > **Notes:**

---

## Area 3 — Application Landscape

1. Can you walk us through the services that make up this application — not the infrastructure they run on, but the actual services and what each one does? Any diagram? Share screen and walk through it — if not, build it live.

   > **Notes:**

2. What are the major application components and their responsibilities?

   > **Notes:**

3. Which workloads remain monolithic or VM-based, and which have been modernized into microservices?

   > **Notes:**

4. How are services deployed per consumer? Does each consumer get their own set of service pods/instances/DB/infra isolation, or is there shared compute with logical isolation?

   > **Notes:**

5. How many applications, services, microservices, or deployable components exist approximately?

   > **Notes:**

6. How are microservices grouped — for example, by product, business capability, team, AKS cluster, or namespace?

   > **Notes:**

7. Which components are customer-facing, internally facing, batch-based, or integration-oriented?

   > **Notes:**

8. Which applications are stateful, and where is their state stored?

   > **Notes:**

9. Are there scheduled jobs, background workers, file-processing pipelines, or asynchronous consumers that are operationally important?

   > **Notes:**

10. Walk me through how a new consumer environment is provisioned today.

    > **Notes:**

---

## Area 4 — Infrastructure Landscape

1. Can you walk us through the HighQ Azure Landing Zone that hosts the platform — what infra services are shared and what are dedicated to a customer's business landing zone? Any diagram?

   > **Notes:**

2. Are any services global or shared across all regions?

   > **Notes:**

3. What Azure subscriptions, management groups, resource groups, and landing zones support the platform?

   > **Notes:**

4. Which Azure services are used in addition to the already known AKS, VMs, Service Bus, Blob Storage, Application Gateway/WAF, Azure SQL, and Key Vault?

   > **Notes:**

5. Which infrastructure components are shared across applications, environments, or customers?

   > **Notes:**

6. How many AKS clusters and VM-based application groups exist?

   > **Notes:**

7. How are AKS workloads separated — clusters, namespaces, node pools, subscriptions, or another model?

   > **Notes:**

8. Which components are managed by the platform team and which by application teams, central IT, or third parties?

   > **Notes:**

9. What Azure Regions are being used? Are there legal, industrial, governance, or other compliance/regulatory requirements?

   > **Notes:**

10. Do you have any compliance requirements within the Azure Infrastructure — legal, industry-specific compliance, ISO?

    > **Notes:**

11. Do you have any internal security requirements?

    > **Notes:**

12. Do you have a defined Security Baseline?

    > **Notes:**

---

## Area 5 — Regions and Environments

1. What do the approximately 12 production environments represent — customers, countries, products, primary/DR pairs, or another segmentation?

   > **Notes:**

2. Is the architecture consistent across regions and environments?

   > **Notes:**

3. What material differences exist between regions, customers, or deployment generations?

   > **Notes:**

4. How do you release application/infra changes required for a single client?

   > **Notes:**

5. Which region or environment is the most representative of the platform?

   > **Notes:**

6. Which one is the most operationally problematic?

   > **Notes:**

7. Are non-production environments architecturally representative of production?

   > **Notes:**

---

## Area 6 — Availability and Disaster Recovery

1. Which components are deployed active-active, active-passive, or without regional redundancy?

   > **Notes:**

2. How are primary and DR environments paired?

   > **Notes:**

3. What triggers failover, and is it automatic or manual?

   > **Notes:**

4. What are the expected RTO and RPO for critical services?

   > **Notes:**

5. Are DR environments continuously running and monitored, or activated only during recovery?

   > **Notes:**

---

## Area 7 — Deployment and Configuration Model

1. How are applications and infrastructure deployed and configured?

   > **Notes:**

2. Is infrastructure consistently managed through IaC, or are some environments manually maintained?

   > **Notes:**

3. Are deployment pipelines and configuration patterns shared across regions and applications?

   > **Notes:**

4. How frequently are production changes released?

   > **Notes:**

5. Can application or infrastructure changes be correlated with operational degradation and incidents?

   > **Notes:**

6. Within a new release version, how do you support appropriate observability changes, if required?

   > **Notes:**

---

## Area 8 — Architecture-Related Operational Risks

1. Which components or dependency paths generate the most operational problems?

   > **Notes:**

2. Which failure scenarios are difficult to diagnose today?

   > **Notes:**

3. Are there components whose health cannot currently be determined with confidence?

   > **Notes:**

4. Which components cause the greatest number of alerts or operational escalations?

   > **Notes:**

5. Are there failures currently discovered by users or customers before Operations detects them?

   > **Notes:**

---

## Expected Workshop Outputs

By the end of this session, the following should be captured or confirmed:

- [ ] Confirmed platform and assessment boundaries
- [ ] High-level application and infrastructure map
- [ ] Initial service and dependency inventory
- [ ] Confirmed region and environment model
- [ ] Primary/DR relationship understood
- [ ] Named architecture and service owners
- [ ] List of required diagrams, inventories, repositories, and access requests
- [ ] Open questions and assumptions requiring validation later
