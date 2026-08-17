# Adobe Experience QA plugin

This plugin is read-only against Adobe. It creates or updates the Confluence QA
report only after the QA run and never modifies or transitions Jira.

Before use, verify `qa-orchestrator` can call the Adobe capability-discovery tool
and that the required product tools are exposed to the authenticated organization.
The Gateway only surfaces entitled tools; missing tools are not a reason to guess.

Credentials must come from Coworker managed secrets or environment configuration.
Never commit IMS tokens, client secrets, or bearer tokens.
