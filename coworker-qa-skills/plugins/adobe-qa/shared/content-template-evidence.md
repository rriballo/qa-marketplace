# Content Template Evidence Contract

Use the existing read-only AJO App Builder MCP to inspect stored Content
Templates. This supplements Adobe CX Gateway evidence; it does not replace a
current configured-message read or rendered Campaign preview.

## Evidence classes

Classify retrieved content before using it:

1. `CONFIGURED_MESSAGE`: current message content returned from an explicit
   Journey/Campaign message read or Campaign preview. This is the strongest
   available content evidence.
2. `EXPLICIT_TEMPLATE_REFERENCE`: the configured object contains the exact
   Content Template ID/reference and that ID is re-read. This proves the source
   association, but not current byte-equivalence after the template was copied
   into a message and edited.
3. `NAME_MATCHED_SOURCE_TEMPLATE`: one stored template was selected only because
   its exact name matches a resolved delivery, Campaign, or Journey name. This is
   source-template evidence only and does not prove linkage or current content.

Never describe class 2 or 3 as rendered proof or current configured-message
content. Include the class, template ID, exact matched name, ETag/request ID when
returned, and retrieval tool in every material QA1 comment.

## Mandatory email discovery

When email is applicable and both Content Template tools are exposed, this phase
is mandatory. Do not skip it because current message content, a brief, or a
preview is missing. Before the list call, tell the user which Journey, Campaign,
or delivery name is being searched. A normal trace is:

```text
Let me search for content templates related to this journey by name.
```

Then call `ajo_content_list_templates`, select an allowed result as described
below, and call `ajo_content_get_template`. A successful list without the detail
call is not Content QA. If either call fails, populate the affected QA1 rows as
`BLOCKED` with the attempted query, tool, and error; do not omit the rows or
continue as though Content QA passed.

## Deterministic discovery

1. If the configured object exposes an exact template ID/reference, call
   `ajo_content_get_template` directly and classify it as
   `EXPLICIT_TEMPLATE_REFERENCE`.
2. Otherwise build exact candidate names in this order: selected message/delivery
   label, resolved Campaign name, resolved Journey name. Remove duplicates and do
   not invent normalized aliases.
3. For each candidate in order, call `ajo_content_list_templates` with
   `filters: [{"field":"name","operator":"==","value":"<exact name>"}]`.
   Inspect the raw Adobe response and follow every returned continuation cursor
   with `start` until no next page remains. Do not assume a stable list envelope.
   Deduplicate repeated IDs and verify returned names exactly, even when Adobe
   accepted the `==` filter.
4. At the first candidate name with results, select only one unambiguous exact
   result. Returned channel metadata may remove candidates that are explicitly
   non-email, but missing channel metadata cannot be treated as non-email. If
   multiple eligible exact results remain, stop and report all IDs as ambiguous.
   Do not continue to a lower-priority name or silently select the first result.
5. If exact searches return no result, derive a distinctive substring from the
   known name, without inventing a different campaign identity, and call
   `ajo_content_list_templates` with
   `filters: [{"field":"name","operator":"~","value":"<substring>"}]`.
   For example, Journey `JRN_PULL_OPPORTUNITY_POC` may use
   `PULL_OPPORTUNITY`.
6. A `~` result may be selected only when, after exhausting pagination and
   deduplicating IDs, exactly one eligible email template remains and its full
   returned `name` exactly equals one of the known message/delivery, Campaign, or
   Journey names from step 2. The fuzzy query locates the object; the returned
   full-name equality validates it. Multiple results, or one result whose full
   name does not exactly match a known name, remain `BLOCKED` and must not be
   auto-selected.
7. Re-read the selected ID with `ajo_content_get_template` immediately before QA.
   Use `data.qa` for normalized fields and retain the raw response for fields or
   variants not normalized by the server.

## Required Content QA

After `ajo_content_get_template` succeeds, inspect the returned normalized and
raw content before report creation. At minimum:

- inventory subject, HTML, text, headers, channel, template type, source shape,
  and every raw email variant returned;
- inspect HTML/CSS for fonts, margins, layout declarations, preheader, images,
  `alt`/`title`, links, UTM parameters, social URLs, mirror/unsubscribe markers,
  and personalization tokens/fallback syntax;
- record exact observed values, missing properties, URLs, and token snippets as
  evidence without exposing secrets or unnecessary personal data;
- map every applicable `Delivery QA - Email` row returned by template preflight
  to `PASS`, `FAIL`, `BLOCKED`, or `NA` with an adjacent QA1 comment.

Source-visible checks must be evaluated; do not leave them `BLOCKED` merely
because the template is source evidence. Rendering, reachability, delivery-time
evaluation, linkage, proof, and recursive-content checks remain `BLOCKED` unless
stronger evidence exists. Content QA is incomplete if the template detail was
retrieved but its HTML/raw fields were not inspected or any applicable email row
was omitted.

## Permitted conclusions

`data.qa` may expose `templateId`, `name`, `templateType`, `channels`, `subject`,
`html`, `text`, `headers`, and `sourceShape`. The raw Adobe object may expose
additional fields.

A stored template can support objective source checks for subject/content
presence, HTML/CSS structure, fonts and margins visible in source, image
`src`/`alt`/`title`, links and UTM values, social URLs, mirror/unsubscribe tokens,
personalization tokens, and preheader/header/sender fields when actually present.
It cannot by itself prove:

- that a name-matched template is linked to the Journey/Campaign;
- that copied and subsequently edited message content still matches the template;
- delivery-time personalization correctness or fallback evaluation;
- recursively resolved fragment or Decision Policy content;
- asset availability, link reachability, desktop/mobile rendering, proof delivery,
  or inbox behavior;
- completeness of variants beyond the normalized first email variant.

Use source-template evidence to `PASS` or `FAIL` only an objective property that
is directly visible in that source. Mark configured-message, rendered, recursive,
or expected-design assertions `BLOCKED` unless stronger evidence exists. A missing
required property directly visible in the selected source may `FAIL`; an absent
field that the API may simply not expose is `BLOCKED`.

## Campaign preview

When `ajo_campaign_preview_content` is exposed and authorized, resolve the exact
Campaign/message scope first, provide only approved test identities/attributes,
and treat Adobe's returned preview as `CONFIGURED_MESSAGE` evidence for that
specific input. The operation requires an inbound IMS user token and Manage
Simulate Content permission. Failure or absence blocks preview-dependent checks;
never send a proof or infer all-profile behavior from one preview.
