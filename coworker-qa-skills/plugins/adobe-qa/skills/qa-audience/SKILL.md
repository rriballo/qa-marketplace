---
name: qa-audience
description: Audit an Adobe Experience Platform audience used by an Adobe Journey Optimizer journey or campaign against a brief, taxonomy, identity, consent, exclusion, and count requirements. Read-only.
---

# Audit audience

Read `shared/mcp-capability-map.md` first and use only exposed read-only Adobe
tools. Use `search_audiences` with its continuation token until the relevant
audience is resolved. Use `preview_audience_membership` only as a bounded preview,
never as proof of all production membership. Use
`inspect_audience_evaluation_jobs` and `inspect_audience_export_jobs` for
freshness and activation health. If a required tool or field is absent, return
`BLOCKED` with the exact attempted scope.

Follow `shared/qa-execution-contract.md`. If the resolved journey/campaign proves
that it has no audience, read-audience, segment, or audience-qualification
relationship, return one `NA` result for every existing Audience QA row with that
same evidence-backed reason. Do not search for an unrelated audience and do not
leave the Audience QA table blank. If an audience reference exists but cannot be
resolved, its rows are `BLOCKED`, not `NA`.

Check label/taxonomy, description, definition, schema, attributes, inclusion and
exclusion logic, consent, blacklist, evaluation state, refresh behavior, identity
namespace, audience counts, and exact linkage to the Journey/Campaign. Require
`HashedKOCID` for Core/Prospect Profile unless the brief explicitly specifies
another namespace.

Return atomic results using `shared/qa-result-contract.md`. If the definition,
counts, identity, or linkage cannot be inspected, report `BLOCKED`. Do not infer
semantic equivalence from matching names.

For reporting, map every applicable result to the exact `Audience QA` row label
returned by `inspectConfluenceQaTemplate`. Produce one QA1 comment per row with
the observed value and evidence locator. Do not combine multiple checklist rows
into a section summary and never write QA2.
