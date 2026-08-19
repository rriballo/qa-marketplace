# Deterministic QA Execution Contract

Use this contract for a complete QA run from an Adobe Journey Optimizer journey
or campaign. It defines the behavior that produced the approved Confluence QA1
report and takes precedence over optional narrative summaries.

## Accepted input

At least one Adobe object source is required:

- an Adobe journey or campaign ID/URL that a connected MCP can resolve; or
- a complete or partial journey/campaign payload supplied by the user.

`jira_ticket`, `brief_source`, taxonomy, screenshots, and proof links improve the
QA but are not prerequisites. Never stop solely because they are absent. Classify
only the checks that require the missing evidence as `BLOCKED`.

## Evidence precedence

1. Re-read runtime Adobe objects and linked dependencies through exposed tools.
2. Use the user-supplied payload for fields not exposed through those tools.
3. Use the brief/SRS, Jira, taxonomy, and approved channel standards as expected
   values when available.
4. Use the checklist's explicit standards for objective requirements such as a
   populated description, `HashedKOCID`, descriptive labels, labelled
   transitions, consent/blacklist hygiene, and removal of test recipient values.
5. Never use an assumption as observed evidence.

For each material result, cite an object ID plus field path, linked-object ID,
tool result, brief location, or supplied-payload path. Re-read a runtime object
before making detailed claims. If runtime and supplied payload disagree, use the
newer authoritative runtime value and report the conflict.

## Object and dependency discovery

1. Identify `journey`, `campaign`, or `delivery` from returned object fields, not
   only from the caller's label.
2. Record organization, sandbox, object ID, version, lifecycle state, validation
   state, timestamps, owner/modifier, dates, timezone, merge policy, identity
   namespace, entry mode, and re-entry configuration when exposed.
3. Traverse the graph or returned relationships to inventory events, audiences,
   conditions, waits, splits, messages/actions, surfaces, content, and ends.
4. Resolve every linked audience, channel configuration, message/content object,
   and proof that exposed tools can read. Follow pagination and re-read the exact
   selected objects.
5. Treat a fixed recipient override, test address, empty message/content ID,
   unexpected safety branch, or other production-risk test configuration as a
   `FAIL`, even when no brief is present.

## Status decision order

Apply this order independently to every template row:

1. `NA`: the resolved object proves the feature is absent or irrelevant. Example:
   no audience relationship, no wait, no percentage split, or no variants.
2. `BLOCKED`: the feature/check is applicable, but a required object, expected
   value, field, permission, brief, renderer, or tool is unavailable.
3. `FAIL`: observed evidence contradicts the brief or an explicit checklist or
   safety standard.
4. `PASS`: observed evidence proves the requirement. A configured value alone
   does not pass a brief-dependent check when the expected value is unavailable.

Use `PASS` only with evidence. Use `NA` only from positive structural evidence,
not because a tool is missing. A section may contain a mixture of all statuses.

## Baseline objective checks

Evaluate these when the corresponding fields are exposed:

- Empty journey/campaign/delivery labels or descriptions fail rows that require
  populated, descriptive values.
- Compare delivery channel/category/surface/tracking with the brief when present;
  otherwise report only objectively observable settings and block the comparison.
- A literal recipient override or test address on a deployed/production-intended
  message is a failure unless the brief explicitly approves it.
- Profile timezone, timezone, dates, re-entry, timeout, and error settings pass
  only against a checklist default or supplied expected value.
- Activity labels must be descriptive and free of obvious placeholder or typo
  values.
- Core/Prospect identity must use `HashedKOCID` unless the brief explicitly
  approves another namespace.
- Transition connectivity is structural; expected route is brief-dependent.
  Empty required transition labels fail the transition-label requirement.
- Consent and blacklist hygiene must be visible where the checklist requires
  them. If graph access is incomplete, block instead of failing absence.
- A message/action reference must identify the selected content sufficiently to
  re-read it. Empty IDs or unresolved references fail or block according to the
  observed configuration.
- Content, links, personalization, accessibility, and preview checks remain
  `BLOCKED` unless content or rendered evidence is actually available. When a
  stored Content Template is available, follow
  `shared/content-template-evidence.md`: source-visible properties may be checked,
  but configured-message/linkage/render assertions require stronger evidence.

## Content evidence workflow

For each applicable email delivery:

1. Prefer a current configured-message read or authorized Campaign preview.
2. If the configured object exposes an exact Content Template ID, re-read that ID
   with `ajo_content_get_template`.
3. Otherwise perform the exhaustive exact-name discovery in
   `shared/content-template-evidence.md`, including all pagination. Never select a
   partial or duplicate match automatically.
4. Record the evidence class in every affected QA1 comment. A name-matched source
   may establish only properties directly observed in its stored source; it does
   not establish current Journey/Campaign linkage or post-copy equality.
5. Inspect normalized `data.qa` plus the preserved raw Adobe response. Block
   unsupported variants, recursive references, asset/link reachability, rendering,
   proof, and delivery-time personalization rather than guessing.

## Template-to-report workflow

1. Call `getConfluenceCapabilities`; require all safety flags in
   `shared/mcp-capability-map.md`.
2. Call `inspectConfluenceQaTemplate` on the runtime template.
3. Populate every returned overview field. Use `Not supplied`, `Not exposed`, or
   `N/A - <reason>` rather than inventing values.
4. Classify every returned row in every relevant Journey/Campaign, Delivery, and
   Audience QA section. For a structurally absent dependency such as no audience,
   populate all rows in that existing section as `NA` with the same precise
   reason. Do not leave the section blank.
5. Use exact section and `whatToQa` labels returned by preflight.
   Never map campaign-core checks into `Journey QA`. A campaign run is incomplete
   when preflight exposes no suitable Campaign QA section.
6. Create a new page with one `createConfluenceQaReport` call. Do not duplicate
   and patch, use the legacy task-ID operation, or submit a replacement body.
7. Re-read both source template and created report in storage format. Verify:
   every requested overview value is present; every submitted QA1 row has an
   adjacent non-empty comment; QA1 is complete only for `PASS`; the number of
   completed QA1 tasks equals the number of `PASS` results; and every QA2 task
   status/comment is byte-equivalent to the source template.
8. A create response without this verification is not a completed QA run.
9. For a Journey only, when complete Coworker `type: "flow"` graph data is
   available and `addConfluenceJourneyDiagram` is exposed, call that dedicated
   tool after report verification. Re-read the report and verify exactly one
   managed Journey diagram attachment appendix. Never invoke it for Campaigns;
   Campaign diagram applicability is `NA`.

Screenshot evidence is optional unless the brief requires it. Append screenshots
only when both a capture capability and a dedicated template-safe upload/append
operation are exposed. Without them, keep the atomic report unchanged and mark
the corresponding preview/evidence rows `BLOCKED`; never use a generic full-body
update merely to add headings.

Return the report URL, final decision, status counts, verification counts, top
failures, and blocked evidence scopes. Never modify Adobe or Jira.
