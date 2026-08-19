# Content Template Evidence

The installable source of truth is
`plugins/adobe-qa/shared/content-template-evidence.md`.

For email QA, prefer current configured-message or Campaign-preview evidence,
then an explicitly referenced Content Template ID. Use exhaustive, paginated,
exact-name discovery only when no ID is available. Select only one unambiguous
exact result and re-read its ID; never choose the first, fuzzy, or duplicate
result.

If exact lookup fails, the installable contract allows a substring query only
when one eligible result remains and its returned full name exactly equals a
known Journey/Campaign/delivery name. For applicable email, list/detail retrieval,
content inspection, and complete `Delivery QA - Email` QA1 coverage are mandatory
when the tools are exposed.

An exact name match is `NAME_MATCHED_SOURCE_TEMPLATE` evidence. It can support
objective checks directly visible in stored source, but it cannot prove linkage,
post-copy message equality, rendering, recursive content, proof delivery, or
delivery-time personalization.
