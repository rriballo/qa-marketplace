# QA Result Contract

Every atomic check returns `PASS`, `FAIL`, `BLOCKED`, or `NA`, plus expected,
observed, evidence locator, severity, comment, and remediation.

- `PASS` requires observed data and a source citation.
- `FAIL` contradicts the brief or standard.
- `BLOCKED` means the required tool, permission, object, field, or evidence is
  unavailable. Never infer a pass.
- `NA` requires an explicit reason.
- Do not expose profile PII; use masked test labels.

Final decision:

```text
APPROVED     = no BLOCKED, no FAIL, and no blocker/high finding
NOT_APPROVED = one or more FAIL or blocker/high findings
BLOCKED      = a required check is BLOCKED and a complete decision is impossible
```
