# Adobe Experience QA plugin

This plugin is read-only against Adobe. It creates or updates the Confluence QA
report only after the QA run and never modifies or transitions Jira.

Before use, verify `qa-orchestrator` can call the Adobe capability-discovery tool
and that the required product tools are exposed to the authenticated organization.
The Gateway only surfaces entitled tools; missing tools are not a reason to guess.
The exact baseline and optional custom tool mapping is in
`shared/mcp-capability-map.md`; skills must mark unsupported checks `BLOCKED`.

The configured Atlassian MCP must be the hardened `MCP-JJRA` deployment with
template-preserving Confluence updates. An unguarded `updateConfluencePage`
operation is not safe for QA reports because it can replace the entire template
body. If the guarded capability is unavailable, the report write must be marked
`BLOCKED` rather than using a generic full-body update.

Credentials must come from Coworker managed secrets or environment configuration.
Never commit IMS tokens, client secrets, or bearer tokens.
