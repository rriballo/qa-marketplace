# QA MCP Template Format

The Confluence page identified by `template_page` is authoritative. The supplied
12-page PDF export is the structural reference. Duplicate that page and keep
its body; never generate a replacement page from Markdown.

## Fixed order

1. `QA - MCP Template` introduction and pre-QA checklist notice.
2. Journey/Campaign Overview and Market Overview tables.
3. Audience Overview table.
4. `Delivery QA - Email`, when email is applicable.
5. `Delivery QA - Push Notification`, when push is applicable.
6. `Delivery QA - Web Browser Push Notification`, when browser push is applicable.
7. `Proof Client Approval`, when proof approval is applicable.
8. `Journey QA`, when a journey is applicable.
9. `Journey QA - Web Browser Push`, when that journey channel is applicable.
10. `Journey IP Warm Up QA`, when IP warm-up is applicable.
11. `Campaign QA - Email`, when a campaign is applicable.
12. `Audience QA`, when an audience is applicable.
13. Append `Content Screenshot`.
14. Append `Journey/Campaign Configuration Screenshot`.

Preserve the existing tables, columns, labels, checkboxes, comments fields,
links, and instructional text. Do not remove a non-applicable section; populate
its QA1 rows as `N/A` with the structural reason. Never redesign, reorder, or
replace the template.

Populate the existing two-column overview tables semantically by section and
field label. Required destinations for this workflow are `Journey Overview`,
`Market Overview`, and `Audience Overview`. Use the exact fields returned by
`inspectConfluenceQaTemplate`; do not depend on placeholder macros. If evidence
for a required field is unavailable, write `BLOCKED: <reason>` in its value cell
rather than inventing a value.

Record results in the existing validation and comments cells. Keep the template's
two QA columns and use `PASS`, `FAIL`, `BLOCKED`, or `N/A`. Unsupported checks are
`BLOCKED` with the missing capability and evidence scope in the adjacent comments
cell. For the current run, write the result to the QA1/1st QA checkbox and its
adjacent comments cell. See `shared/qa-cell-mapping.md`. Do not add a new
results table or status section, and do not place row findings only below the
table.

Every automated checklist row must contain an existing QA1 checkbox and adjacent
QA1 Comments cell. If template inspection reports either missing, the template
must be corrected once in Confluence before the agent can safely populate that
row. Keep QA2 present and unchanged.

When dedicated capture and template-safe append/upload capabilities are exposed,
embed the rendered content screenshot first and the full journey/campaign
configuration screenshot second. If unavailable, mark the relevant evidence
rows `BLOCKED` and do not use a generic body replacement to add placeholders.
