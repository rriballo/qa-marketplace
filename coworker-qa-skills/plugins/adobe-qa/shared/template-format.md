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
links, and instructional text. Conditional selection may leave a non-applicable
section collapsed or empty, but must not redesign, reorder, or replace it.

Record results in the existing validation and comments cells. Keep the template's
two QA columns and use `PASS`, `FAIL`, `BLOCKED`, or `N/A`. Unsupported checks are
`BLOCKED` with the missing capability and evidence scope in the adjacent comments
cell. For the current run, write the result to the QA1/1st QA checkbox and its
adjacent comments cell. See `shared/qa-cell-mapping.md`. Do not add a new
results table or status section, and do not place row findings only below the
table.

At the end, embed the rendered content screenshot first and the full
journey/campaign configuration screenshot second. If either is unavailable,
retain its heading and write `BLOCKED` with the attempted capability.
