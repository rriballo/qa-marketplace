# Adobe CX Coworker QA Skills

Coworker marketplace plugin source for read-only quality assurance of Adobe
Journey Optimizer journeys, campaigns, audiences, and message deliveries.

## Installation

Add this repository as a Coworker marketplace, then install the `adobe-qa` plugin.
The plugin layout mirrors the existing `ajo-coworker-marketplace` repository:
`.claude-plugin` manifests, plugin-scoped `.mcp.json`, and skills under
`plugins/adobe-qa/skills`.

The original `skills/` directory remains as portable source documentation. The
installable plugin is under `plugins/adobe-qa`.

## Entry point

Run `qa-execute` for an end-to-end QA1. Supply at least one of:

- an Adobe journey/campaign ID or URL resolvable by the connected MCP
- a complete or partial journey/campaign payload

Optional context:

- `jira_ticket`: Jira issue key or URL
- `brief_source`: brief, SRS, taxonomy, or linked requirements
- `adobe_object`: Journey, Campaign, audience, or delivery URL/identifier/payload
- `qa_type`: `journey`, `campaign`, or `delivery`
- `template_page`: optional existing Confluence QA template page override; the
  default is the configured `QA - MCP Template` page

Jira, brief, taxonomy, and `qa_type` are not execution prerequisites. Their
absence produces `BLOCKED` only for dependent comparisons. The skill creates a
Confluence report only after every preflight row has been classified, then
re-reads it to verify QA1 coverage and that QA2 is unchanged. It does not update
or transition Jira.

## Conditional sections

The orchestrator resolves the Adobe object and dependencies before selecting
sections. A journey with email includes Audience (when referenced),
Journey/Campaign, Email Delivery, and applicable Proof/Preview/Personalization.
Email plus SMS includes both delivery sections. A journey without an audience
populates the existing Audience QA rows as `N/A` with the structural reason.
Applicability is tracked in the QA evidence data and does not alter
the Confluence template structure. Screenshots are appended only when dedicated
capture and template-safe upload/append capabilities are exposed.
The exact checklist order and table rules are defined in
`plugins/adobe-qa/shared/template-format.md` from the supplied PDF reference.

## Outcome policy

`PASS`, `FAIL`, `BLOCKED`, and `N/A` are distinct states. A report is approved only
when there are no unresolved blockers, failures, or blocked checks.

## Required connections

- Adobe CX Coworker Gateway: entitled campaign, channel, audience, identity, and
  merge-policy reads are discovered at runtime.
- Existing AJO App Builder MCP (`AJO_MCP_URL`): read-only Content Template,
  Campaign resolution/preview, fragment, and audience-definition evidence. Keep
  Adobe write gates and Content user-token fallback disabled for QA.
- Hardened Atlassian MCP: scoped Confluence read/write access for the report.
  Jira and generic attachment operations are not exposed. A dedicated
  Journey-only tool renders Coworker flow JSON and embeds the PNG safely.

## Known limitations

The skill reports unsupported or unavailable operations as `BLOCKED`. It does not
claim to simulate profile eligibility, prove live delivery, render every message,
inspect every journey policy reference, or validate Adobe Campaign unless the
connected MCP advertises those capabilities. It never changes Adobe objects.
Stored templates selected only by exact name are source evidence and cannot prove
that copied Journey/Campaign message content still matches them.
For applicable email, `qa-execute` must search/retrieve the Content Template when
the tools are exposed, inspect the returned source, and populate every
`Delivery QA - Email` QA1 row. A controlled substring search is accepted only
when the single result's full name exactly matches a known object name.
