---
name: qa-delivery
description: Audit an Adobe Journey Optimizer delivery or message configuration and content against a brief, channel guidelines, personalization requirements, tracking, accessibility, and preview evidence. Read-only.
---

# Audit delivery

Read `shared/mcp-capability-map.md` and
`shared/content-template-evidence.md`, and use only tools actually exposed. Use
`ajo_channel_configuration_list` and `ajo_channel_configuration_get` for
channel metadata, then re-read the linked campaign/delivery before reporting
fields. The documented CX Coworker Gateway does not expose message HTML, subject
lines, personalization tokens, offer content, or rendered proofs. Supplement it
with the existing AJO MCP when Content Template, Campaign resolution, or Campaign
preview tools are exposed. Check label, description, category,
surface, tracking, profile email fields, sender, subject, preheader, fonts,
margins, images and labels, links and UTM parameters, social URLs, mirror page,
unsubscribe, personalization syntax/fallbacks, and desktop/mobile preview when
the connection supports it.

For every applicable email, prefer current configured-message or authorized
Campaign-preview evidence. Otherwise use an explicit configured template ID. If
none is exposed, run the exhaustive exact-name and pagination workflow in
`shared/content-template-evidence.md`; never select the first, fuzzy, or duplicate
result. Re-read the exact selected ID immediately before QA. State
`CONFIGURED_MESSAGE`, `EXPLICIT_TEMPLATE_REFERENCE`, or
`NAME_MATCHED_SOURCE_TEMPLATE` in affected comments.

A stored template can pass/fail objective source-visible properties such as
subject/content presence, HTML/CSS, image attributes, source URLs/UTMs, required
tokens, and visible personalization syntax. A name match cannot pass linkage,
current copied-message equality, rendering, link reachability, recursive
fragment/policy content, asset availability, proof, or delivery-time evaluation.
Mark those assertions `BLOCKED` without stronger evidence. Inspect both normalized
`data.qa` and raw variants; do not assume the normalized first variant is complete.

Also inspect delivery/action fields embedded in a supplied journey or campaign
payload. Cite the action ID and field path. Apply
`shared/qa-execution-contract.md`: a fixed literal/test recipient override,
empty required label/description, or unresolved message/content reference is an
objective failure when exposed. Comparisons to intended category, surface,
sender, subject, content, or design remain `BLOCKED` without their expected or
rendered evidence.

Missing image label is a blocker in the supplied QA standard. Missing title or
alt text is a finding unless the brief makes it a blocker. Do not claim a link is
working or a preview is correct without evidence. Never send a live proof.

For reporting, map each result to the exact applicable `Delivery QA - <channel>`
row returned by `inspectConfluenceQaTemplate`. Produce a separate QA1 status and
evidence-backed comment for every applicable row, including `BLOCKED` rows.
Never put the findings only below the table and never write QA2.
