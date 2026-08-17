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

Check label/taxonomy, description, definition, schema, attributes, inclusion and
exclusion logic, consent, blacklist, evaluation state, refresh behavior, identity
namespace, audience counts, and exact linkage to the Journey/Campaign. Require
`HashedKOCID` for Core/Prospect Profile unless the brief explicitly specifies
another namespace.

Return atomic results using `shared/qa-result-contract.md`. If the definition,
counts, identity, or linkage cannot be inspected, report `BLOCKED`. Do not infer
semantic equivalence from matching names.
