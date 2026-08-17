---
name: qa-report
description: Create an evidence-backed Confluence QA report from Adobe QA results, brief requirements, and the existing QA template. Use after QA checks complete. Never modify or transition Jira.
---

# Create QA report

Use the Atlassian MCP only for the report and its evidence. Read
`shared/mcp-capability-map.md` and `shared/template-format.md` before writing.
The default hardened Atlassian MCP is Confluence-only: it does not read Jira
and does not upload attachments. Duplicate the
runtime `template_page`, or the governed default
`https://wundertracker.atlassian.net/wiki/spaces/~62fcacec2cbfba0566aca9fb/pages/16267575322/QA+-+MCP+Template`,
in the approved existing space. The duplicated page is the report: do not
recreate it from Markdown and do not replace it with the local fallback
template.

Before updating, call the connected Confluence MCP's read-only
`getConfluenceCapabilities` tool. Require
`safety.template_preserving_confluence_update: true` and
`safety.arbitrary_template_body_replacement: false`. If the capability tool is
unavailable, the safety values are not present, or the server rejects the
proposed structure, do not retry with a generic full-body update; report the
write as `BLOCKED`.

## Template preservation is mandatory

- Keep the exact template structure, headings, tables, order, formatting, and
  existing instructional text.
- Do not add, remove, rename, reorder, or merge template sections.
- Populate only existing template fields, table cells, and designated content
  placeholders. If a selected QA result has no matching region, place it in the
  existing Findings, Technical Details, or Evidence region; do not add a new QA
  heading.
- Do not insert an Applicability and Coverage section into the Confluence page.
  Keep the applicability record in the QA evidence/result data and summarize it
  only within existing template regions.
- Do not convert the page to a new Markdown layout or overwrite the template
  body wholesale.

The required section order and channel-specific checklist content are defined in
`shared/template-format.md`, based on the supplied 12-page PDF. The Confluence
template's existing table cells are the only result destinations.

After all existing template content, append exactly these evidence sections, in
this order:

1. `Content Screenshot`
2. `Journey/Campaign Configuration Screenshot`

Embed or attach the actual screenshots only when `capture_evidence` and an
explicit attachment-capable Atlassian tool are both exposed. The content
screenshot must show the rendered message/content. The journey/campaign
screenshot must show the orchestration/configuration view. With the baseline
MCP, keep both required headings and write `BLOCKED`, naming the missing tool
and evidence scope. Never claim a placeholder is a screenshot.

Preserve `PASS`, `FAIL`, `BLOCKED`, and `NA` exactly. Include the source citation
for every finding. Include a limitations section whenever the Adobe Gateway or
custom AJO MCP cannot expose a requested check. Do not silently omit blocked
checks. Do not update Jira fields, comments, or status.

Use the orchestrator applicability record to decide which existing template
regions to populate. Do not create new sections for included areas. A selected
area must retain all of its `BLOCKED` checks; an omitted area must be recorded
with its reason in the existing Findings or Technical Details region.
