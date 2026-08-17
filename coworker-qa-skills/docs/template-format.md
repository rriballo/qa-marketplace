# QA MCP Template Format

The Confluence page identified by `template_page` is the authoritative template.
The supplied PDF export is the structural reference. A QA run must duplicate
that page and preserve its body; this document defines the required order and
does not authorize generating a replacement page.

## Fixed order

1. `QA - MCP Template` introduction and pre-QA checklist notice.
2. Journey/Campaign Overview and Market Overview tables.
3. Audience Overview table.
4. `Delivery QA - Email` checklist, when email is applicable.
5. `Delivery QA - Push Notification` checklist, when push is applicable.
6. `Delivery QA - Web Browser Push Notification` checklist, when browser push is applicable.
7. `Proof Client Approval` checklist, when proof approval is applicable.
8. `Journey QA` checklist, when a journey is applicable.
9. `Journey QA - Web Browser Push` checklist, when that journey channel is applicable.
10. `Journey IP Warm Up QA` checklist, when IP warm-up is applicable.
11. `Campaign QA - Email` checklist, when a campaign is applicable.
12. `Audience QA` checklist, when an audience is applicable.
13. Append `Content Screenshot`.
14. Append `Journey/Campaign Configuration Screenshot`.

The first three overview areas and every selected checklist retain the PDF's
existing tables, columns, labels, checkboxes, comments fields, links, and
instructional text. Conditional selection may leave a non-applicable existing
template section collapsed or empty, but must not redesign, reorder, or replace
the structure.

## Checklist rules

- Keep the two QA columns (`1st QA`/`2nd QA`, or `QA1`/`QA2` where the template
  uses those labels), their checkboxes, and their comments columns.
- Record `PASS`, `FAIL`, `BLOCKED`, or `N/A` in the existing check/comment cells;
  do not add a new results table or status section.
- Preserve the supplied descriptions verbatim. Add observed values, evidence,
  and remediation to the existing comments cells.
- Use the exact channel-specific checklist matching the resolved channel. Do not
  substitute an email checklist for push or browser push.
- For an unsupported check, write `BLOCKED` in the existing validation cell and
  the missing capability/evidence scope in the adjacent comments cell.
- At the end, embed the rendered content screenshot first and the full
  journey/campaign configuration screenshot second. If unavailable, retain the
  headings and write `BLOCKED` with the attempted capability.
