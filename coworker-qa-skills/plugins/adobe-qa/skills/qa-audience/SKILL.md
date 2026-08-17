---
name: qa-audience
description: Audit an Adobe Experience Platform audience used by an Adobe Journey Optimizer journey or campaign against a brief, taxonomy, identity, consent, exclusion, and count requirements. Read-only.
---

# Audit audience

Use only read-only Adobe operations. Call capability discovery first when the
session does not already contain its guidance. Use `search_audiences` and retrieve
the individual audience if that operation is exposed. Use evaluation/export job
tools to validate freshness and activation health; do not treat a count estimate
as proof of membership correctness.

Check label/taxonomy, description, definition, schema, attributes, inclusion and
exclusion logic, consent, blacklist, evaluation state, refresh behavior, identity
namespace, audience counts, and exact linkage to the Journey/Campaign. Require
`HashedKOCID` for Core/Prospect Profile unless the brief explicitly specifies
another namespace.

Return atomic results using `shared/qa-result-contract.md`. If the definition,
counts, identity, or linkage cannot be inspected, report `BLOCKED`. Do not infer
semantic equivalence from matching names.
