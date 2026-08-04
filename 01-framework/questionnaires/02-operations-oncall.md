# Workshop Questionnaire — Operations & On-Call Teams

**Audience:** SRE, operations engineers, on-call responders, NOC (if applicable)  
**Duration:** 60 min  
**Goal:** Understand incident response workflow, operational tooling, and process gaps

---

## Section 1 — Incident Detection

1. How are you notified when something goes wrong in production? (alert, consumer call, Slack, email?)
2. What percentage of incidents are detected by your monitoring vs. reported by consumers?
3. What is the typical time between a problem starting and you being notified? (MTTD estimate)
4. Which consumer environments / regions generate the most incidents?
5. Are there recurring incidents that keep coming back? What are they?

---

## Section 2 — On-Call & Escalation

6. Is there a formal on-call rotation? How is it structured?
7. What tool handles on-call scheduling and paging — PagerDuty, OpsGenie, Teams, something else?
8. What is the escalation path for a P1 incident? Walk me through it step by step.
9. How are consumers notified when there is a production incident affecting their environment?
10. Is there a status page for consumers?
11. How long is the on-call handoff, and what information is transferred?

---

## Section 3 — Incident Response

12. When you are paged, what is your first action?
13. Is there a war room / incident bridge process for P1s?
14. Do runbooks exist for the most common alerts? Are they easy to find and follow?
15. How often are runbooks updated? Who is responsible?
16. Can you give me an example of a recent P1 or P2 — what happened, how was it resolved?
17. What is the typical MTTR for a P1? For a P2?

---

## Section 4 — Post-Incident

18. Is there a formal post-incident review (PIR) / root cause analysis (RCA) process?
19. Are PIR action items tracked to completion? In what tool?
20. Are recurring incidents analyzed to find systemic patterns?

---

## Section 5 — Operational Tooling

21. What tools does the operations team use daily? (List them)
22. Is there a single pane of glass — one place to go to understand overall platform health?
23. How do you differentiate between a platform problem and a consumer-specific problem?
24. Is there SLA reporting to consumers? How is it generated?
25. How do you handle scheduled maintenance — communication, monitoring suppression?

---

## Section 6 — Gaps & Wishlist

26. What is the biggest operational pain point today?
27. If you could fix one thing about the current observability setup, what would it be?
28. Are there environments or services where you feel blind?
29. Has there ever been a major incident that could have been prevented with better monitoring?
