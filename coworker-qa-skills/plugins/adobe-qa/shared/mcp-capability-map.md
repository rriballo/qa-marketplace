# MCP Capability Map

Use this map as an execution contract. Discover the connected tools at runtime,
then use only tools that are actually exposed. A configured tool that is absent,
denied, or returns insufficient fields is `BLOCKED`, not `PASS` or `NA`.

A complete journey/campaign payload supplied by the user is valid read-only
evidence for fields it contains. Tool absence blocks only unavailable fields and
dependencies; it does not make supplied graph fields inaccessible. Runtime
re-reads take precedence when newer or authoritative.

## Adobe CX Gateway baseline

The documented baseline is read-only and entitlement-dependent:

| Tool | Use for | Cannot prove by itself |
|---|---|---|
| `ajo_campaign_list` | Locate supported AJO campaigns and paginate inventory | Journey graph, message rendering, proof approval |
| `ajo_campaign_get` | Re-read campaign metadata, targeting, schedule, channel, and available content metadata | Complete journey logic or message HTML |
| `ajo_channel_configuration_list` | Locate channel configurations | Whether a specific message renders correctly |
| `ajo_channel_configuration_get` | Re-read channel configuration metadata | Subject/body/personalization correctness |
| `search_audiences` | Locate audiences and paginate results | Definition correctness or membership proof from a name/count |
| `preview_audience_membership` | Preview membership for a supplied audience/query scope | Guaranteed production membership or profile eligibility |
| `inspect_audience_evaluation_jobs` | Check audience evaluation freshness/status | Audience semantic correctness |
| `inspect_audience_export_jobs` | Check activation/export job status | Delivery or rendered-content correctness |
| `search_identity_namespaces` | Verify available identity namespaces | That a journey uses the required identity |
| `search_merge_policies` | Inspect merge-policy metadata | Complete profile policy behavior unless fields are returned |

The Gateway baseline does not expose complete journeys, scripts, message HTML,
subject lines, personalization tokens, offers, rendered previews, screenshots,
live proofs, profile simulation, or write operations. Do not invent replacement
tool names for these checks.

## Optional custom Adobe MCP

Use these only when the connected server explicitly exposes them:

- `read_journey_graph`: journey activities, transitions, waits, events, splits,
  conditions, scripts, and end-to-end graph logic.
- `read_message_content`: message HTML, subject, preheader, links, assets, and
  personalization data.
- `validate_personalization`: token syntax and fallback validation.
- `render_preview`: desktop/mobile or channel previews.
- `capture_evidence`: actual content or journey configuration screenshots.

Custom tools do not change the read-only policy. A write, send, publish, or
activation operation is outside this QA package.

## Connected Atlassian MCP

The hardened `MCP-JJRA` endpoint is scoped to Confluence space
`~62fcacec2cbfba0566aca9fb` / `13985677353` and exposes these tools:

- `getConfluenceCapabilities`: call before any write; require
  `template_preserving_confluence_update: true` and
  `arbitrary_template_body_replacement: false`.
- `getConfluencePage`: read template/page storage body and version.
- `inspectConfluenceQaTemplate`: required preflight for a new report. It returns
  recognized Journey, Market, and Audience Overview fields, QA sections,
  exact checklist row labels, QA1 readiness, and template defects.
- `createConfluenceQaReport`: preferred atomic report write. Send compact
  overview field values and QA1 row results. The server validates every mapping
  before creating the page, updates native Confluence tasks and adjacent QA1
  Comments, and preserves QA2 unchanged.
- `duplicateConfluencePage`: duplicate the approved template server-side.
- `updateConfluenceQaOverviewFields`: semantic overview update for an existing
  report, matched by section and field label.
- `updateConfluenceTemplateFields`: preferred QA write; send page ID and
  placeholder/value replacements only. The server fetches the large body,
  applies targeted replacements, validates structure, and writes the new
  version.
- `updateConfluenceChecklistRows`: preferred checklist write; send compact
  section/row/status/comment records. The server writes the existing QA1
  checkbox and adjacent QA1 comments cell in each matched row.
- `updateConfluencePage`: update an existing page only after the server guard
  accepts unchanged structure, replacement of existing placeholder wrappers by
  cell text, and the two final screenshot sections.
- `addConfluenceJourneyDiagram`: Journey-only idempotent operation. It accepts
  bounded Coworker `type: "flow"` JSON, renders a PNG server-side, uploads or
  updates `journey-configuration-diagram.png`, and manages one final Journey
  diagram appendix while preserving QA tables and QA2. Its schema rejects
  Campaign requests.
- Other scoped read/comment/page tools may be used only for evidence needed by
  the report.

This endpoint does **not** expose Jira tools or generic Confluence attachment
tools. Its only attachment capability is the dedicated Journey flow renderer.
If other screenshots cannot be uploaded, append the required screenshot headings with
`BLOCKED` and state the missing capability. Never use a generic full-body update
or create a replacement page from Markdown.

Do not pass a 94 KB body through the client when targeted fields are sufficient.
The MCP does not expand `${file:...}` references inside string parameters. Use
`updateConfluenceTemplateFields`; if full-body update is unavoidable, pass the
actual storage-format string to `updateConfluencePage`, never a filesystem path.

For new reports, do not use the legacy `createConfluenceJourneyQaReport` task-ID
workflow. It cannot express semantic overview fields and QA1 row comments as one
validated operation. Require capability safety values
`semantic_template_preflight`, `qa1_only_enforced`, and `qa2_preserved`.

## Status rules

- `PASS` requires returned evidence and a citation.
- `FAIL` requires returned evidence contradicting the brief or QA standard.
- `BLOCKED` means the tool, permission, field, object, or evidence is unavailable.
- `NA` means the check is genuinely irrelevant to the resolved configuration.
- Re-read the object/dependency before making detailed claims and follow every
  continuation cursor/token until exhausted or explicitly bounded.
