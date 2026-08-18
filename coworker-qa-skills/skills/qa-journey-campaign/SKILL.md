# Journey and Campaign QA

Inspect the complete orchestration graph and compare it with the extracted
requirements. This skill applies to AJO Journeys and Adobe Campaign workflows or
campaigns; use `area: journey` or `area: campaign` accordingly.

A supplied journey/campaign payload is valid evidence. Missing brief values block
only comparisons that require them. Apply the deterministic status and reporting
rules in `plugins/adobe-qa/shared/qa-execution-contract.md`.

## Properties

- Label and taxonomy
- Descriptive description
- Entry event or read-audience configuration
- Re-entry policy
- Timezone and profile-timezone setting
- Start date and end date
- Frequency/recurrence
- Timeout and error handling, normally default unless briefed otherwise
- Active/draft/published status appropriate for QA

## Graph and activities

- Every activity label is descriptive.
- Correct orchestration activity is selected.
- Exact audience or event is selected.
- Identity type is correct, normally `HashedKOCID` for Core/Prospect Profile.
- Wait durations, wait-until dates, time windows, and quiet hours match the brief.
- Events use the required behavior and payload fields.
- Date/time conditions use the correct days, hours, timezone, and boundary logic.
- Percentage splits total 100% and match the brief.
- Variants and decisioning branches match the brief.
- Scripts compile, reference valid fields, handle nulls, and do not contain test
  values or unsafe fallback logic.
- Every transition is connected to the expected next activity and has the right
  label.
- Consent check and blacklist exclusion are present where required.
- Correct delivery/message is selected on each action.
- No unexpected safety condition, branch, or dead end exists.
- Journey/campaign logic matches the business narrative end to end.

## Technical evidence

Capture the object URL/ID, version, status, last modified timestamp, graph
snapshot, activity IDs, linked audience/delivery IDs, scripts, and any dependency
or external integration references. If the graph cannot be traversed completely,
return `BLOCKED` for the affected checks.
