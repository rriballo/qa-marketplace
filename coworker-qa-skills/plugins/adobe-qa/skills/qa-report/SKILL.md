---
name: qa-report
description: Create an evidence-backed Confluence QA report from Adobe QA results, brief requirements, and the existing QA template. Use after QA checks complete. Never modify or transition Jira.
---

# Create QA report

Use the Atlassian MCP only for the report and its evidence. Read
`shared/mcp-capability-map.md`, `shared/template-format.md`, and
`shared/qa-cell-mapping.md` before writing. For an end-to-end run, also read and
enforce `shared/qa-execution-contract.md`.
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
`safety.arbitrary_template_body_replacement: false`,
`safety.semantic_template_preflight: true`,
`safety.qa1_only_enforced: true`, and `safety.qa2_preserved: true`. If the capability tool is
unavailable, the safety values are not present, or the server rejects the
proposed structure, do not retry with a generic full-body update; report the
write as `BLOCKED`.

Call `inspectConfluenceQaTemplate` on the runtime template page before assembling
the write payload. If it reports a missing QA1 checkbox/comments pair for an
applicable row, stop and report the exact template correction required. Use its
exact section, overview-field, and checklist-row labels; never guess a repeated
`Label` or `Description` row.

Build the payload from the preflight inventory, not only from findings. For each
included Delivery, Audience, and Journey QA section, classify every returned row
as `PASS`, `FAIL`, `BLOCKED`, or `NA` and send it to the report operation. An
unpopulated applicable row is an incomplete QA run.

When an existing template section represents a dependency proven absent, fill
all of its rows as `NA` with the structural reason. This keeps the QA1 report
auditable and prevents an empty section from looking untested.

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
- Replace only the contents of existing `<ac:placeholder>` cells. The hardened
  MCP permits removing those placeholder wrappers while preserving the table
  structure. Do not pass `${file:...}` or a filesystem path as the body; the
  update argument must contain the actual storage-format string.
- Prefer `updateConfluenceTemplateFields` for placeholder population. Send a
  compact array of exact placeholder/value pairs and let the MCP fetch and
  update the large storage body server-side. Use `updateConfluencePage` only
  for an approved final screenshot appendix or another change that cannot be
  represented as targeted field replacements.
- For a new report, prefer one `createConfluenceQaReport` call instead of
  duplicating and patching the page in multiple steps. Send all resolved
  `overview` records for Journey Overview, Market Overview, and Audience
  Overview, plus all QA1 `rows`. The tool must validate every mapping before it
  creates the page. Do not use the legacy `createConfluenceJourneyQaReport`.
- For an existing report, use `updateConfluenceQaOverviewFields` for overview
  values and `updateConfluenceChecklistRows` for QA1 results.
- For QA results, use the existing checklist rows as the source of truth. For
  every applicable atomic result, locate the matching section and checklist row
  using `shared/qa-cell-mapping.md`, then populate the current run's QA1/1st QA
  checkbox and adjacent Comments cell. Do not append findings below the table.
- Prefer `updateConfluenceChecklistRows` for QA results. Send one compact row
  record per check with `section`, `whereToQa` when needed, exact `whatToQa`,
  `status`, and `comment`. The MCP will match the existing row and populate QA1
  Check and QA1 Comments server-side. If a row is missing or ambiguous, the
  tool must fail rather than writing a summary below the table.
- Send results in bounded batches if the row payload becomes large; never send
  the full 94 KB storage body just to update checkbox/comment cells.
- `PASS` means QA1 checkbox `* [x]` plus a pass reason and citation in QA1
  Comments. `FAIL`, `BLOCKED`, and `NA` mean QA1 checkbox `* [ ]` plus the reason,
  expected/observed detail where relevant, and citation in QA1 Comments.
- Do not populate QA2/2nd QA. This agent performs QA1 only. A summary below the
  table is optional and cannot substitute for row-level
  checkbox and comment values.

After creation, re-read the report in storage format. Verify every requested
overview value, QA1 task status, and adjacent QA1 comment, and confirm that QA2
task statuses and comments match the source template. A create response alone
is not completion evidence.

Count the submitted statuses and stored QA1 tasks. The stored completed-QA1 count
must equal the submitted `PASS` count; every submitted row must have a non-empty
adjacent comment; and the source/report QA2 status and comment sequences must be
identical. Return these verification counts with the page URL.

The required section order and channel-specific checklist content are defined in
`shared/template-format.md`, based on the supplied 12-page PDF. The Confluence
template's existing table cells are the only result destinations.

When both screenshot capture and a dedicated template-safe upload/append
capability are exposed, append exactly these evidence sections, in this order:

1. `Content Screenshot`
2. `Journey/Campaign Configuration Screenshot`

Embed or attach the actual screenshots only when `capture_evidence` and an
explicit attachment-capable Atlassian tool are both exposed. The content
screenshot must show the rendered message/content. The journey/campaign
screenshot must show the orchestration/configuration view. With the baseline
MCP, do not perform a second generic body update just to add headings; keep the
atomic report unchanged and mark the relevant preview/evidence checklist rows
`BLOCKED`. Never claim a placeholder is a screenshot.

Preserve `PASS`, `FAIL`, `BLOCKED`, and `NA` exactly. Include the source citation
for every finding. Include a limitations section whenever the Adobe Gateway or
custom AJO MCP cannot expose a requested check. Do not silently omit blocked
checks. Do not update Jira fields, comments, or status.

Use the orchestrator applicability record to decide which existing template
regions to populate. Do not create new sections for included areas. A selected
area must retain all of its `BLOCKED` checks; an omitted area must be recorded
with its reason in the existing Findings or Technical Details region.
