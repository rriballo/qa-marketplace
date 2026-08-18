# Adobe Experience QA plugin

This plugin is read-only against Adobe. It creates or updates the Confluence QA
report only after the QA run and never modifies or transitions Jira.

Use `qa-execute` for a complete journey/campaign QA1. It accepts either a
runtime-resolvable Adobe object or a supplied object payload. Jira, a brief, and
taxonomy are optional evidence; their absence blocks only dependent rows. The
deterministic workflow is defined in `shared/qa-execution-contract.md`.

Before use, verify `qa-execute` can call the Adobe capability-discovery tool
and that the required product tools are exposed to the authenticated organization.
The Gateway only surfaces entitled tools; missing tools are not a reason to guess.
The exact baseline and optional custom tool mapping is in
`shared/mcp-capability-map.md`; skills must mark unsupported checks `BLOCKED`.

The configured Atlassian MCP must be the hardened `MCP-JJRA` deployment with
template-preserving Confluence updates. An unguarded `updateConfluencePage`
operation is not safe for QA reports because it can replace the entire template
body. If the guarded capability is unavailable, the report write must be marked
`BLOCKED` rather than using a generic full-body update.

The deployed endpoint must also expose `inspectConfluenceQaTemplate` and
`createConfluenceQaReport`, with `semantic_template_preflight`,
`qa1_only_enforced`, and `qa2_preserved` set to `true`. New reports use this
semantic QA1-only workflow; the legacy task-ID report tool is not sufficient
because it does not populate adjacent QA1 comments.

Credentials must come from Coworker managed secrets or environment configuration.
Never commit IMS tokens, client secrets, or bearer tokens.
