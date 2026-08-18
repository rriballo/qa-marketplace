# Verified Execution Example

Use this as a behavioral example, not as expected values for another object.

For journey `JRN_PULL_OPPORTUNITY_POC` / ID
`badee425-1407-4b4f-a43b-42b537c926d0`, the supplied graph contained one unitary
event, one condition with two branches, one email action, and no audience, wait,
percentage split, or variants. The runtime template preflight returned 19
overview fields and 58 QA1 rows across Delivery Email, Audience, and Journey.

The verified report classified all 58 rows:

- `PASS`: 4
- `FAIL`: 9
- `BLOCKED`: 28
- `NA`: 17

The objective failures included empty delivery label/description, a fixed email
recipient override, weak activity labels, `CRMID_AJO_POC` instead of the required
`HashedKOCID`, empty transition labels, absent visible hygiene rules, an empty
message ID, and production-risk test configuration. Brief/content-dependent rows
were `BLOCKED`. All Audience QA rows and absent optional structures were `NA`.

Storage-format verification found 58 populated QA1 comments, 4 completed QA1
tasks, and zero QA2 changes. A future run must derive its own counts from its own
evidence, but it must meet the same coverage and verification invariants.
