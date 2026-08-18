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
4. Preflight the live template and classify every returned QA1 row.
5. Populate structurally absent dependencies as `NA`, including every Audience
   QA row when no audience relationship exists.
6. Use one atomic `createConfluenceQaReport` call.
7. Re-read and verify all overview values and QA1 comments, completed QA1 equals
   PASS count, and QA2 is unchanged.

Return the report URL, status counts, verification counts, top failures, and
blocked evidence scopes.
