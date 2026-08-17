# Deployment Checklist

1. Add this repository as a Coworker marketplace and install `adobe-qa`.
2. Configure the Adobe CX Coworker Gateway or existing custom AJO MCP in the
   plugin-scoped `.mcp.json`. Keep IMS credentials in managed secrets only.
3. Call the Adobe capability discovery operation or inspect exposed tools. Verify
   one known audience and campaign can be read before enabling QA decisions.
4. Bind the Atlassian MCP connection and verify Confluence template duplication and
   page update in a test space.
5. The default Confluence template is `https://wundertracker.atlassian.net/wiki/spaces/~62fcacec2cbfba0566aca9fb/pages/16267575322/QA+-+MCP+Template`.
   Override it with `template_page` only when a different approved template is
   required.
6. Configure the approved Confluence space and parent page. Do not guess these.
7. Run against a draft/test Adobe object and inspect the evidence links before
   using the skill for production QA.
8. Confirm that preview validation never sends live email or SMS.
9. Confirm the Jira write policy remains disabled. This package creates the
   Confluence report only and does not update or transition Jira.
10. Verify a test run produces an Applicability and Coverage table and omits
    only genuinely non-applicable sections; unsupported checks inside included
    sections must remain `BLOCKED`.

## First test invocation

```text
Run qa-orchestrator for:
- Jira ticket: <key or URL>
- Brief/SRS: <URL or attached document>
- Adobe object: <Journey/Campaign URL>
- QA type: journey or campaign
- Confluence template: <existing template page URL>
Use the draft/test environment and do not send messages.
```
