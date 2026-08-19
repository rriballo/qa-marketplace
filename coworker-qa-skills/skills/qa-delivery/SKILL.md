# Delivery QA

Inspect every delivery/message selected by a Journey or Campaign and compare it
with the brief, taxonomy, channel guidelines, and content requirements.

Inspect embedded action fields in supplied journey/campaign payloads. Fixed test
recipient overrides, empty required labels/descriptions, and unresolved content
references are objective failures when exposed. Missing rendered content or
expected brief values makes only the corresponding checks `BLOCKED`.

When the installable plugin exposes the existing AJO Content MCP, follow
`plugins/adobe-qa/shared/content-template-evidence.md`. Prefer current
configured-message or Campaign-preview evidence, then an explicit template ID.
Use exhaustive exact-name template discovery only as source evidence; follow all
pagination and never auto-select fuzzy or duplicate matches. A name-matched
stored template cannot prove current Journey/Campaign linkage, post-copy message
equality, rendering, proof, or delivery-time personalization.

## Configuration checks

- Label and description
- Marketing versus transactional category
- Channel surface
- Open and click tracking settings
- Core profile email field and prospect email fallback, when applicable
- From address and from name
- Subject line and subject personalization
- Pre-header

## Content checks

- Font family and obvious font inconsistencies
- Required margins and responsive layout
- No placeholder or missing images
- Every image has a label; missing label is a blocker. Missing title or alt text
  is a finding but not a blocker unless the brief says otherwise.
- Links have expected URL, target, link type, and UTM parameters.
- UTM values match the market, campaign, channel, source, medium, and taxonomy.
- Social links resolve to active pages for the correct market.
- Mirror page exists and is not empty.
- Unsubscribe link exists and is not empty.
- Personalization expressions compile and have a valid fallback.
- Preview renders on desktop and mobile without broken layout, missing content,
  or unresolved tokens.

Copy every tracked URL into evidence and report the exact failing URL rather than
only saying that tracking is incorrect. Do not send a proof to a real recipient.
