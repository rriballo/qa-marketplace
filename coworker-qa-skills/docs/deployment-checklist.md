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
10. Verify a test run preserves the duplicated template structure exactly and
    populates proven non-applicable rows as `N/A`; unsupported checks remain
    `BLOCKED`. Append screenshots only when dedicated capture and template-safe
    upload/append capabilities are exposed.
11. Compare the resulting page against `docs/template-format.md`: the original
    checklist order and table structure must be unchanged. When actual
    screenshots are supported, they must be appended in the documented order.
12. Redeploy the hardened `MCP-JJRA` service before testing. The server-side
    template guard must reject a replacement body and allow only existing-cell
    updates, placeholder-wrapper removal, plus the two final screenshot
    sections. Verify that `${file:...}` is not passed as a literal body value.
13. Verify `getConfluenceCapabilities` exposes `inspectConfluenceQaTemplate`,
    `createConfluenceQaReport`, and safety values `semantic_template_preflight`,
    `qa1_only_enforced`, `qa2_preserved`, and `journey_diagram_only`.
14. Run template inspection against page `16267575322` and require an empty
    `issues` array. On 2026-08-18, the Journey wait-activity and Custom checks
    rows were repaired with native QA1/QA2 task controls and the live template
    preflight passed. If a later template edit introduces an issue, stop report
    creation and repair only the reported row through the restricted MCP tool.
15. Create a disposable QA report and verify Journey, Market, and Audience
    Overview values; Delivery, Audience, and Journey QA1 checkboxes/comments;
    and unchanged QA2 cells by re-reading the created page.
16. The 2026-08-18 semantic smoke tests passed for overview values, PASS,
    FAIL, BLOCKED, and N/A QA1 behavior, adjacent comments, and QA2 preservation.
17. For a Journey test, pass a valid Coworker `type: "flow"` graph to
    `addConfluenceJourneyDiagram`. Verify one PNG attachment and one final
    `Journey Configuration Diagram` appendix, then retry and verify no duplicate.
    Confirm the schema rejects `objectType: "campaign"`.
18. On 2026-08-18, Cloud Run revision `atlassian-public-mcp-00019-xbs`
    rendered the supplied 9-node/8-edge Journey flow to an 1800x337 PNG. A retry
    retained one heading and one attachment reference, Campaign input was
    rejected, and the disposable page was deleted successfully.
    A full-coverage run also populated all 19 recognized overview fields and all
    58 Delivery Email, Audience, and Journey QA1 rows with zero QA2 changes. The
    disposable pages were deleted after verification.

## First test invocation

```text
Run qa-execute for:
- Adobe object: <Journey/Campaign URL, ID, or supplied payload>
- Jira ticket: <optional key or URL>
- Brief/SRS: <optional URL or attached document>
- QA type: <optional journey or campaign hint>
- Confluence template: <existing template page URL>
Use the draft/test environment and do not send messages.
```

17. Acceptance requires one QA1 result/comment per preflight row, completed QA1
    count equal to PASS count, and byte-equivalent QA2 statuses/comments between
    the source template and created report.
