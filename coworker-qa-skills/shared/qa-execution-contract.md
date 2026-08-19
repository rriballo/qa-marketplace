# Deterministic QA Execution Contract

The installable contract is
`plugins/adobe-qa/shared/qa-execution-contract.md`. A complete QA run:

1. Accepts a resolvable Adobe object ID/URL, a supplied journey/campaign payload,
   or both.
2. Treats Jira, brief, taxonomy, proof, and screenshots as optional evidence.
   Missing evidence blocks only dependent checks.
3. Re-reads runtime objects and dependencies, then uses supplied payload fields
   where the connection cannot expose them.
4. Uses the installable Content Template evidence contract. Name-matched stored
   content is source evidence only, not proof of configured-message linkage.
   For applicable email, template discovery/detail, source inspection, and every
   `Delivery QA - Email` QA1 row are mandatory when the tools are exposed.
5. Applies status order `NA`, `BLOCKED`, `FAIL`, `PASS` independently per row.
   `NA` requires proven structural absence; `PASS` requires observed evidence.
6. Populates every preflight row, including all Audience QA rows as `NA` when the
   resolved object proves no audience relationship exists.
7. Creates the report atomically with `createConfluenceQaReport`.
8. Re-reads the report and verifies overview values, QA1 comments, completed QA1
   count equals PASS count, and QA2 is unchanged.

Use the installable contract as the full source of truth.
The installable field map is `plugins/adobe-qa/shared/ajo-qa-field-map.md`.
