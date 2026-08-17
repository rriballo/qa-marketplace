---
name: qa-orchestrator
description: Run an evidence-backed, read-only QA of an Adobe Journey Optimizer journey, supported campaign, audience, and delivery, then create a Confluence report. Use when the user asks to QA, validate, proof, or approve a configured Adobe customer journey or campaign.
---

# Orchestrate Adobe QA

Remain read-only against Adobe. Do not repair, publish, approve, activate, send,
or delete Adobe resources.

1. Call `ajo_get_capabilities` when the connected custom AJO server exposes it.
   Otherwise use the documented CX Coworker Gateway tool set and its known
   limitations. Use exact tool names and schemas; never invent operations.
2. Read the Jira ticket, linked brief/SRS, taxonomy, and existing Confluence QA
   template. Confirm the sandbox and permissions.
3. Resolve the supplied Adobe object and every referenced audience, delivery,
   channel, content, and proof dependency. Re-read each resource before
   reporting detailed fields. Follow pagination exactly for inventories.
4. Read `shared/section-selection.md` and build the applicability record before
   running checks. Select sections from resolved object evidence, not `qa_type`.
   Include one delivery section per channel found and include
   Proof/Preview/Personalization when an included delivery or brief requires it.
   Omit only genuinely absent sections and record the reason.
5. Run audience checks with `search_audiences` and related RT-CDP tools when
   available. Run campaign checks with `ajo_campaign_list` and `ajo_campaign_get`.
   Run journey-graph and message-content checks only when a custom connected MCP
   explicitly exposes those capabilities.
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
   Do not update or transition Jira.

## Required input

Ask for `jira_ticket`, `brief_source`, `adobe_object`, and `qa_type` if any are
missing. `template_page` is optional because a governed default exists.

## Blind spots to state in the report

State any applicable limitations: no profile eligibility simulation, no guaranteed
live delivery proof, no complete journey/campaign policy inspection, no message
rendering/proof capability, no Adobe Campaign support, or no Confluence write
capability. The exact limitations depend on runtime capabilities and permissions.
