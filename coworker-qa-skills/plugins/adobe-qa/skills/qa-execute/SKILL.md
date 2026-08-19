---
name: qa-execute
description: Execute an end-to-end, read-only QA1 of an Adobe Journey Optimizer journey or campaign from a runtime object or supplied payload, resolve its dependencies, and create a verified Confluence checklist report. Use when the user asks to run, perform, test, or generate QA for a journey or campaign.
---

# Execute Adobe QA1

Read these contracts before doing anything:

- `shared/qa-execution-contract.md`
- `shared/ajo-qa-field-map.md`
- `shared/mcp-capability-map.md`
- `shared/qa-result-contract.md`
- `shared/qa-cell-mapping.md`
- `shared/template-format.md`
- `shared/content-template-evidence.md`

Remain read-only against Adobe and Jira. The only permitted write is the
governed Confluence QA report.

## Run

1. Accept an Adobe journey/campaign ID or URL, a supplied object payload, or both.
   Do not require Jira, a brief, taxonomy, or `qa_type` before starting. Ask for
   clarification only when no Adobe object can be identified. Missing comparison
   evidence becomes row-level `BLOCKED`.
2. Discover the Adobe and Confluence MCP capabilities. Use only exposed tools.
   Resolve and re-read the target and all accessible audience, delivery, channel,
   content, and proof dependencies. If complete journey data is supplied directly,
   inspect it even when `read_journey_graph` is not exposed.
3. Build an evidence inventory with exact object IDs and field paths. Determine
   applicable channels and whether audience, waits, splits, variants,
   personalization, and other optional features are actually present.
   For each applicable email, resolve Content evidence using the precedence and
   deterministic discovery rules in `shared/content-template-evidence.md`.
   Record the evidence class; do not present a name-matched source template as
   the current configured message.
4. Call `inspectConfluenceQaTemplate` before classifying rows. Use the returned
   overview fields, sections, and exact `whatToQa` values as the output schema.
   For a campaign, require an exact Campaign QA section for campaign-core rows;
   never write them into Journey QA. If none is returned, state that the template
   cannot support a complete campaign QA and do not claim completion.
5. Apply `shared/qa-execution-contract.md` and delegate the domain evaluation to
   `qa-journey-campaign`, `qa-delivery`, and `qa-audience` as applicable. Produce
   exactly one `PASS`, `FAIL`, `BLOCKED`, or `NA` result and one evidence-backed
   comment for every preflight row in the selected report scope.
6. If no audience is linked, populate every existing Audience QA row as `NA` and
   state that the resolved journey/campaign contains no audience relationship.
   Apply the same rule to absent optional row-level features. Do not omit or leave
   those rows blank.
7. Populate Journey Overview, Market Overview, and Audience Overview from runtime
   evidence. Use explicit missing-value text; do not infer market, country, or
   language without identifying the derivation in the value.
8. Call `createConfluenceQaReport` once with all overview records and QA1 rows.
   Use the governed template unless the user supplied an override. Never call
   `createConfluenceJourneyQaReport` or use a generic body replacement fallback.
9. Re-read the source template and created report in storage format. Verify full
   row coverage, non-empty adjacent QA1 comments, PASS-to-completed-checkbox
   equality, overview values, and unchanged QA2. If verification fails, report
   the run as incomplete and do not claim success.
10. Journey only: when the resolved Journey evidence includes Coworker graph
    output with `type: "flow"` and the Confluence MCP advertises
    `addConfluenceJourneyDiagram`, call it after report verification with
    `objectType: "journey"`, the report page ID, and the complete flow object.
    Re-read the report and verify one final `Journey Configuration Diagram`
    appendix containing `journey-configuration-diagram.png`. Do not call this tool
    for Campaigns. A Campaign diagram is `NA`, not `BLOCKED`.

## Output

Return the Confluence URL, decision, counts for all four statuses, number of QA1
rows/comments/completed tasks verified, confirmation that QA2 is unchanged, top
failures, and blocked evidence scopes. Human approval remains required.
For a Journey flow, also report whether the generated diagram attachment and
managed appendix were verified.
