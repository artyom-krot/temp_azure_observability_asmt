# Session 1 — Kick-off & Scope Alignment: Questionnaire

**Session:** Workshop 1 — Kick-off & Scope Alignment
**Duration:** ~30 min
**Audience:** Sponsor, Product/Platform leadership, Architect, Ops/SRE lead

---

## Area 1 — Deployment Model: Understand the HighQ Platform Context

1. What exactly constitutes a business landing zone in your platform?

   > **Notes:**

2. Is each client deployed using the same architecture and infrastructure pattern?

   > **Notes:**

3. Which components are client-specific, and which are shared across clients or regions?

   > **Notes:**

4. What differences normally exist between clients: versions, configuration, scale, integrations or monitoring?

   > **Notes:**

5. Is the application architecture materially identical across regions?

   > **Notes:**

6. Are monitoring configuration and alert routing standardized across regions?

   > **Notes:**

---

## Area 2 — Select the Representative Scope

> Which landing zone provides the best combination of: business criticality, observability pain, platform representativeness, evidence availability, and SME availability?

1. Which client or business landing zone is the most business-critical?

   > **Notes:**

2. Which client/region/BLZ currently generates the highest alert volume or causes the greatest operational workload?

   > **Notes:**

3. Which region should be assessed based on production importance, incident history and alert volume?

   > **Notes:**

4. Should we select the most problematic environment, or consider other criteria?

   > **Notes:**

5. Which client/region/BLZ has knowledgeable SMEs available during the assessment?

   > **Notes:**

6. Which client/region/BLZ should be used as the secondary comparison sample?

   > **Notes:**

---

## Expected Kick-off Decisions

The following decisions should be confirmed by the end of this session:

- [ ] Which single application, business service, or landing zone should be used as the **primary assessment scope**?
- [ ] Does it include both application and infrastructure layers?
- [ ] Does it contain a representative mixture of AKS, VM, messaging, data, ingress, and external dependencies?
- [ ] Does it experience enough alerts or incidents to support evidence-based analysis?
- [ ] Is the selected scope small enough for two people to analyze within four weeks?
- [ ] What is explicitly excluded?
- [ ] Can findings from this scope reasonably indicate broader platform patterns, and where would generalization be unsafe?
