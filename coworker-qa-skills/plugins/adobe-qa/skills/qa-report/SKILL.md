---
name: qa-report
description: Create an evidence-backed Confluence QA report from Adobe QA results, brief requirements, and the existing QA template. Use after QA checks complete. Never modify or transition Jira.
---

# Create QA report

Use the Atlassian MCP only for the report and its evidence. Duplicate the
runtime `template_page`, or the governed default
`https://wundertracker.atlassian.net/wiki/spaces/~62fcacec2cbfba0566aca9fb/pages/16267575322/QA+-+MCP+Template`,
in the approved existing space. Preserve its existing structure where the API
permits it, then populate the overview, applicability/coverage table, decision
counts, selected QA sections, findings, remediation, limitations, technical
details, and evidence index.

Preserve `PASS`, `FAIL`, `BLOCKED`, and `NA` exactly. Include the source citation
for every finding. Include a limitations section whenever the Adobe Gateway or
custom AJO MCP cannot expose a requested check. Do not silently omit blocked
checks. Do not update Jira fields, comments, or status.

Render only sections marked included by the orchestrator applicability record.
Always render the applicability/coverage table, including explicit omitted
sections and reasons. A selected section must retain all of its `BLOCKED`
checks; omission is allowed only for a genuinely non-applicable section.
