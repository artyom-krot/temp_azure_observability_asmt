# Context Materials

Place all client-provided materials here before analysis.

## architecture/
Architecture diagrams, network topology, service maps, infrastructure layout.
Accepted formats: PNG, PDF, draw.io, Visio exports, Terraform state exports.

## documentation/
Existing runbooks, operational procedures, onboarding docs, SLA documents,
incident post-mortems, monitoring setup documentation.

## workshops/
Notes from stakeholder workshops and interviews.
Use the questionnaires in `01-framework/questionnaires/` to structure sessions.
Name files: `YYYY-MM-DD_audience_notes.md` (e.g., `2026-08-10_engineering-team_notes.md`)

---

**Important:** When you add a file here, start a Claude session and say:
"Analyze [filename] and extract relevant evidence for the assessment checklist domains."
Claude will map the content to domains and populate `02-discovery/`.
