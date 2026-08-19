# Execute Adobe QA1

Use the installable `plugins/adobe-qa/skills/qa-execute/SKILL.md` as the runtime
skill. It executes a complete read-only QA from a journey/campaign ID, URL, or
supplied payload and writes only the governed Confluence report.

## Required behavior

1. Read `plugins/adobe-qa/shared/qa-execution-contract.md`.
   Use `plugins/adobe-qa/shared/ajo-qa-field-map.md` for field-to-row evidence.
2. Do not require Jira, a brief, taxonomy, or `qa_type`; missing expected values
   make only the dependent rows `BLOCKED`.
3. Resolve and re-read accessible Adobe dependencies, while using supplied
   payload fields as evidence when runtime tools do not expose them.
4. For each email, follow
   `plugins/adobe-qa/shared/content-template-evidence.md`: prefer configured
   content, then explicit template reference, then exhaustive unambiguous exact
   name matching as source-only evidence. If exact lookup fails, permit the
   controlled substring search only when the one returned template's full name
   exactly matches a known object name. Call template detail and inspect its
   content; do not omit Content QA.
5. Preflight the live template and classify every returned QA1 row.
6. Populate structurally absent dependencies as `NA`, including every Audience
   QA row when no audience relationship exists.
   For applicable email, require one result/comment for every preflight
   `Delivery QA - Email` row before report creation.
7. Use one atomic `createConfluenceQaReport` call.
8. Re-read and verify all overview values and QA1 comments, completed QA1 equals
   PASS count, and QA2 is unchanged.
9. Journey only: if Coworker provides complete `type: "flow"` graph data and the
   MCP exposes `addConfluenceJourneyDiagram`, call it after report verification
   and verify the final attachment appendix. Never call it for Campaigns.

Return the report URL, status counts, verification counts, top failures, and
blocked evidence scopes.
