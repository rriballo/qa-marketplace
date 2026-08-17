# QA Result Contract

Every atomic check must return one object with these fields:

```yaml
id: string
area: audience | delivery | journey | campaign | proof | technical
check: string
status: PASS | FAIL | BLOCKED | NA
severity: blocker | high | medium | low | none
expected: string
observed: string
evidence:
  - source: brief | jira | adobe | confluence | external
    locator: string
    detail: string
comment: string
remediation: string
```

## Rules

- `PASS` requires an observed value and evidence locator.
- `FAIL` means the observed configuration contradicts a requirement.
- `BLOCKED` means the required object, field, permission, or evidence could not be
  inspected. Never infer a pass.
- `NA` is allowed only when the requirement is explicitly not applicable and the
  reason is recorded.
- Severity is `blocker` for launch-risk, compliance, broken content, missing
  unsubscribe, missing image label, wrong audience identity, or broken journey
  logic. Use the brief's explicit severity when it differs.
- Do not alter Adobe objects during QA. If a reversible proofing change is
  requested, stop and report it as a required manual action.
- Never expose profile PII in the report. Use masked identifiers or profile test
  labels.

## Final decision

```text
APPROVED     = no BLOCKED, no FAIL, no blocker/high findings
NOT_APPROVED = one or more FAIL findings or blocker/high findings
BLOCKED      = one or more BLOCKED checks and no complete decision is possible
```
