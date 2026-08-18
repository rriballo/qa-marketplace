---
name: qa-orchestrator
description: Run an evidence-backed, read-only QA of an Adobe Journey Optimizer journey, supported campaign, audience, and delivery, then create a Confluence report. Use when the user asks to QA, validate, proof, or approve a configured Adobe customer journey or campaign.
---

# Orchestrate Adobe QA

Remain read-only against Adobe. Do not repair, publish, approve, activate, send,
or delete Adobe resources.

For a full execution request, use `qa-execute` and follow
`shared/qa-execution-contract.md`. This orchestrator coordinates the domain
skills; it must not weaken that deterministic status or verification contract.

1. Read `shared/mcp-capability-map.md` and discover the tools exposed by the
   connected Adobe and Atlassian MCPs. If a custom Adobe server exposes
   `ajo_get_capabilities`, call it; otherwise use the documented baseline. On
   Atlassian, call `getConfluenceCapabilities` before any report write. Use exact
   tool names and schemas; never invent operations.
2. Read the Jira ticket only if one was supplied and a separate Jira-capable MCP
   is connected. The
   default hardened Atlassian MCP is Confluence-only, so use supplied
   `jira_ticket`/brief context and mark Jira retrieval `BLOCKED` when it cannot
   be read. Read the linked brief/SRS, taxonomy, and existing Confluence QA
   template. Read `shared/template-format.md` as the required format contract.
   Confirm the sandbox and permissions.
3. Resolve the supplied Adobe object and every referenced audience, delivery,
   channel, content, and proof dependency. Re-read each resource before
   reporting detailed fields. Follow pagination exactly for inventories.
4. Read `shared/section-selection.md` and build the applicability record before
   running checks. Select sections from resolved object evidence, not `qa_type`.
   Include one delivery section per channel found and include
   Proof/Preview/Personalization when an included delivery or brief requires it.
   Omit only genuinely absent sections and record the reason.
5. Run audience checks with `search_audiences`,
   `preview_audience_membership`, `inspect_audience_evaluation_jobs`, and
   `inspect_audience_export_jobs` when exposed. Run campaign checks with
   `ajo_campaign_list` and `ajo_campaign_get`; use channel configuration tools
   for channel metadata. Run journey-graph and message-content checks only when
   the custom tools in the capability map are explicitly exposed.
6. For each unavailable capability, create a `BLOCKED` result with the missing
   capability and the exact attempted scope. Never turn an unavailable check into
   `PASS` or `N/A`.
7. Preserve source citations for every material conclusion: brief/Jira source,
   Adobe object ID and URL, field path, tool response, or screenshot.
8. Separate structural readiness, current lifecycle status, date eligibility,
   profile eligibility, delivery simulation, and proof approval. Do not claim one
   proves another.
9. Calculate the decision using the shared result contract. Human approval remains
   required; the skill recommends a decision but does not launch anything.
10. Duplicate and populate the existing Confluence template through Atlassian
    MCP. Use `template_page` when supplied, otherwise use
    `https://wundertracker.atlassian.net/wiki/spaces/~62fcacec2cbfba0566aca9fb/pages/16267575322/QA+-+MCP+Template`.
     Treat the duplicated page as immutable structure: do not recreate it from
     Markdown, add/remove/reorder headings, change tables, or insert an
     applicability section. Populate only existing fields/regions. Use the
     applicability record to decide which existing regions receive results, and
     put omitted-section reasons in an existing Findings or Technical Details
     region. At the very end append `Content Screenshot`, followed by
     `Journey/Campaign Configuration Screenshot`; attach/embed the actual images
     or record `BLOCKED` if the runtime cannot capture them.
    Do not update or transition Jira. Do not call attachment tools unless a
     runtime capability discovery explicitly exposes one.
11. The report is QA1-only. Populate Journey Overview, Market Overview, Audience
    Overview, every applicable Delivery QA row, Audience QA row, and Journey QA
    row. Use `inspectConfluenceQaTemplate`, then the atomic
    `createConfluenceQaReport` operation. Re-read the created page and verify
    overview values plus each QA1 checkbox/comment; QA2 must remain unchanged.
12. Do not omit an applicable QA row because Adobe cannot expose its evidence.
    Send it as `BLOCKED` with the missing capability in its adjacent QA1 comment.
    Use `NA` only when the resolved object proves the row is genuinely not
    applicable, such as no wait activity or no personalization.

## Input

Require only an identifiable `adobe_object`: an ID/URL resolvable through an
exposed tool or a supplied journey/campaign payload. `jira_ticket`,
`brief_source`, `qa_type`, taxonomy, and `template_page` are optional. Do not stop
when optional context is missing; mark only dependent checks `BLOCKED`.

## Blind spots to state in the report

State any applicable limitations: no profile eligibility simulation, no guaranteed
live delivery proof, no complete journey/campaign policy inspection, no message
rendering/proof capability, no Adobe Campaign support, or no Confluence write
capability. The exact limitations depend on runtime capabilities and permissions.
