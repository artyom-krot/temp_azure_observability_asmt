# Project Guide — How to Use Claude for This Assessment

A practical reference for running the observability assessment using Claude Code.
Come back here when you forget how something works.

---

## 1. Core Concepts (read this once)

### What Claude Code is
Claude Code is the CLI tool you're running right now (`claude` in your terminal).
It's a conversation — you type, Claude responds, and it can read/write files in your repo.

### What CLAUDE.md is
`CLAUDE.md` (at the root of this repo) is **automatically loaded at the start of every session**.
It tells Claude: who the client is, what the 12 assessment domains are, what standards to apply,
and how to format findings. You never have to re-explain the project context.

**Rule:** If you update the client context or change the scope, update `CLAUDE.md`.
Everything else (findings, notes, discovery data) lives in the folder structure.

### What a "session" is
Every time you open Claude Code, that's a new session. Claude reads `CLAUDE.md` fresh.
It also reads any files you point it at. It does NOT automatically remember your last conversation —
that's why the repo structure matters. **The repo is your persistent memory.**

### What subagents are
Subagents are specialized versions of Claude with focused instructions for a specific job.
Defined in `.claude/agents/`. Claude picks the right one automatically based on what you ask,
or you can invoke one by name. They have the same tools as Claude but narrower scope and better
domain knowledge baked in.

---

## 2. Subagent Quick Reference

Six agents are defined for this project. Here's when to use each one.

| Agent | Invoke when... | What it does | Output |
|---|---|---|---|
| `evidence-analyzer` | You drop a client file into `00-context/` | Reads it, extracts observations, maps to domains | `02-discovery/` |
| `azure-analyst` | You have Azure configs, ARM exports, or CLI access | Deep Azure Monitor / AKS / diagnostic settings analysis | `02-discovery/azure-monitor/` |
| `datadog-analyst` | You have Datadog exports, JSON, or API access | Analyzes monitors, dashboards, APM, module utilization | `02-discovery/datadog/` |
| `gap-analyst` | Discovery phase is done | Runs checklist-vs-evidence gap analysis, scores maturity | `03-analysis/findings/` |
| `findings-writer` | You have raw workshop notes or verbal observations | Converts unstructured input into formatted findings | `03-analysis/findings/` + gap register |
| `workshop-facilitator` | Before OR after a stakeholder workshop | Pre: builds tailored interview guide. Post: processes notes | `00-context/workshops/` |
| `report-synthesizer` | All findings are complete | Writes executive summary, technical report, roadmap | `05-deliverables/` |

### How to invoke an agent

**Automatically (preferred):** Just describe what you need. Claude picks the agent.
```
"Analyze the architecture diagram I just added to 00-context/architecture/"
"I finished the engineering workshop, here are my notes: [paste]"
"Run the gap analysis on everything in 02-discovery/"
```

**Explicitly (when you want a specific agent):**
```
"Use the datadog-analyst agent to analyze this monitor export"
"Use the workshop-facilitator to prepare me for tomorrow's ops team interview"
```

---

## 3. Assessment Workflow — Step by Step

### Phase 1: Framework (done)
- [x] Repo structure created
- [x] `CLAUDE.md` written
- [x] Assessment checklist (`01-framework/assessment-checklist.md`)
- [x] Maturity model (`01-framework/maturity-model.md`)
- [x] Workshop questionnaires (`01-framework/questionnaires/`)
- [x] Subagents defined (`.claude/agents/`)

### Phase 2: Discovery

**When you get a client document:**
```
1. Place the file in the correct 00-context/ subfolder
2. Tell Claude: "Analyze [filename] for the assessment"
   → evidence-analyzer runs, writes to 02-discovery/
```

**Before each workshop:**
```
Tell Claude: "Prepare me for the [engineering / ops / leadership] workshop"
→ workshop-facilitator reads existing discovery and builds a tailored guide
```

**After each workshop:**
```
Tell Claude: "Process these workshop notes: [paste raw notes]"
→ workshop-facilitator writes structured summary to 00-context/workshops/
   and gives you a list of domain observations to feed into findings
```

**When you get Azure CLI access (MCP or direct):**
```
Tell Claude: "Query the Azure environment and analyze the monitoring setup"
→ azure-analyst runs queries and writes to 02-discovery/azure-monitor/
```

**When you get Datadog access:**
```
Tell Claude: "Analyze the Datadog setup — use the API"
→ datadog-analyst queries the API and writes to 02-discovery/datadog/
```

### Phase 3: Gap Analysis

Run when you have meaningful discovery data (doesn't have to be complete — you can iterate):
```
Tell Claude: "Run a gap analysis on the current discovery data"
→ gap-analyst reads 02-discovery/, compares to checklist, writes findings
```

To add a finding from a specific observation:
```
Tell Claude: "Record this as a finding: [describe what you observed]"
→ findings-writer formats and saves it
```

### Phase 4: Reporting

When findings are ready for a deliverable:
```
"Write the executive summary"
"Write the technical report section for alerting"
"Generate the improvement roadmap"
→ report-synthesizer reads 03-analysis/ and writes to 05-deliverables/
```

---

## 4. Example Prompts (copy-paste ready)

### Starting a session
```
"Continue the observability assessment. What's the current state of discovery?"
```
Claude will read `CLAUDE.md` and scan the repo to orient itself.

### After dropping in a file
```
"I've added [filename] to 00-context/architecture/. Analyze it and extract evidence for the assessment."
```

### Workshop prep
```
"Prepare interview questions for tomorrow's workshop with the engineering team.
Focus on what we still don't know after the Azure analysis."
```

### Workshop debrief
```
"Here are my notes from today's engineering team workshop. Process them:
[paste notes]"
```

### Adding a quick finding from conversation
```
"Record a finding: during the workshop the on-call lead said they have no runbooks
for any of the ASB-related alerts. They get paged and figure it out each time."
```

### Running gap analysis
```
"Run the full gap analysis on everything collected so far.
Start with ALERT and AKS domains — those are the most critical."
```

### Checking progress
```
"Show me the current gap register and maturity scores."
```

### Writing a report section
```
"Write the Alerting section of the technical report based on current findings."
```

---

## 5. Repo Structure — Where Things Live

```
CLAUDE.md                   ← Project brain. Claude reads this every session.
HOW-TO-USE.md               ← This file.

00-context/                 ← PUT CLIENT MATERIALS HERE
  architecture/             ← Diagrams, topology maps
  documentation/            ← Runbooks, SLA docs, existing monitoring docs
  workshops/                ← Workshop notes (written by workshop-facilitator)

01-framework/               ← Assessment tools (don't edit unless scope changes)
  assessment-checklist.md   ← 80+ checkboxes across 12 domains
  maturity-model.md         ← 0-5 scoring rubric + scoring sheet
  questionnaires/           ← Interview guides per audience

02-discovery/               ← RAW EVIDENCE (written by agents)
  azure-monitor/            ← Azure Monitor, Log Analytics, AKS configs
  datadog/                  ← Datadog monitors, dashboards, APM, agents
  aks/                      ← AKS-specific findings
  infrastructure/           ← VM configs, networking, ASB
  operations/               ← Process and ops findings

03-analysis/                ← GAP ANALYSIS (written by gap-analyst)
  findings/                 ← One .md per domain (ALERT_findings.md, etc.)
  gap-register.md           ← Master table of all findings

04-recommendations/         ← IMPROVEMENT PLAN (written by report-synthesizer)
  quick-wins.md
  roadmap.md
  implementation-plan.md

05-deliverables/            ← FINAL OUTPUTS
  technical-report.md
  executive-summary.md

.claude/
  agents/                   ← Subagent definitions (6 files)
  settings.json             ← Claude permissions config
```

---

## 6. Key Things to Know as a Claude Newcomer

### Context window
Claude can read a large amount of text in one session, but not infinitely.
For heavy analysis work (e.g., processing many discovery files at once),
Claude may summarize earlier parts of the conversation. This is normal.
The repo structure is designed to avoid this problem — agents read specific files,
not the whole conversation history.

### When to start a new session vs. continue
- **Continue the same session** for related work in the same sitting.
- **Start a new session** when switching between phases (e.g., done with discovery, starting gap analysis).
  New sessions re-read `CLAUDE.md` and have a clean context — better for focused analysis work.

### Claude can be wrong
Claude's analysis is only as good as the evidence you feed it.
If a finding looks wrong: say "this seems off, the actual config is X" and it will correct itself.
Treat Claude as a very fast, knowledgeable analyst who still needs you to validate conclusions.

### Be specific when you can
Vague: *"analyze the monitoring"*
Better: *"analyze the Datadog monitor export at 02-discovery/datadog/monitors-export.json
and identify which monitors are missing runbook links"*

The more specific the question, the more precise the output.

### MCP integrations (coming later)
When you get Azure access, we'll set up the Azure MCP server.
This lets Claude query live Azure configs directly — no manual exports needed.
The `azure-analyst` and `datadog-analyst` agents are already written to use CLI/API queries
when that access is available.

---

## 7. The 12 Assessment Domains (quick reminder)

| # | Code | Domain |
|---|------|--------|
| 1 | INSTR | Instrumentation — are apps emitting the right signals? |
| 2 | COLL | Collection & Pipelines — do signals reach the monitoring platform? |
| 3 | ALERT | Alerting — do the right people get paged for the right reasons? |
| 4 | DASH | Dashboards & Visibility — can teams see what matters? |
| 5 | AKS | AKS Observability — is Kubernetes monitored properly? |
| 6 | MULTI | Multi-Env Consistency — are all 12 environments covered equally? |
| 7 | APM | APM & Distributed Tracing — can you trace a request end-to-end? |
| 8 | SEC | Security Observability — are security events detected? |
| 9 | OPS | Operational Processes — on-call, runbooks, incident response |
| 10 | GOV | Governance — ownership, tagging, cost |
| 11 | AUTO | Automation & CI/CD — is observability config deployed as code? |
| 12 | DD | Datadog Utilization — licensed vs. actively used modules |
