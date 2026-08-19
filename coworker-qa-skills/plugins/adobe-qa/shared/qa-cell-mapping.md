# QA Table Cell Mapping

The Confluence QA template is a checklist, not a narrative report. Every
applicable atomic result must be written into the matching existing table row.
Do not put the result only in a summary, footer comment, or section appended
below the table.

## Required Row Mapping

For each result, resolve:

- `section`, such as `Journey QA`, `Delivery QA - Email`, or `Audience QA`;
- `where_to_qa`, when present;
- `what_to_qa`, the checklist item text;
- `qa_column`, which is `QA1` / `1st QA` for the current run;
- `status`, one of `PASS`, `FAIL`, `BLOCKED`, or `NA`;
- `comment`, a concise reason and evidence citation.

Match the row using the section and checklist text, not only a generic word
such as `Description`, `Status`, or `Label`. If a row cannot be uniquely
resolved, do not guess: record the result as `BLOCKED` and explain the mapping
failure in an existing comments or technical-details area.

Before mapping, call `inspectConfluenceQaTemplate` on the runtime template. Use
the exact section and `whatToQa` strings returned by that preflight. This is
required because repeated labels and continuation rows occur in the template.
If the preflight reports a missing QA1 checkbox/comments pair, stop report
creation and identify the exact template row that requires correction.

## Cell Behavior

The template row layout is:

```text
Where to QA | What to QA | Description | QA1 Check | QA1 Comments | QA2 Check | QA2 Comments
```

- `PASS`: set the QA1 checkbox to `* [x]` and write why it passed plus the
  evidence locator in QA1 Comments.
- `FAIL`: leave QA1 unchecked as `* [ ]` and write the failed observation,
  expected value, severity, and evidence in QA1 Comments.
- `BLOCKED`: leave QA1 unchecked as `* [ ]` and write the missing capability,
  attempted scope, and evidence limitation in QA1 Comments.
- `NA`: leave QA1 unchecked as `* [ ]` and write the explicit non-applicability
  reason in QA1 Comments.

Do not change QA2 unless a second QA run was explicitly requested. Do not
replace the row's `Where to QA`, `What to QA`, `Description`, links, or
instructional text. Do not add a new results table.

The current automation is QA1-only even when a second run is requested. A QA2
workflow requires a separate, explicitly approved server capability; never
simulate it with a full-body update.

## Coverage

Populate every applicable row in each included section, including rows whose
result is `BLOCKED`. Rows that are genuinely non-applicable may remain
unchecked only when the adjacent comment records `NA` and the reason. A summary
of findings may exist, but it is supplementary and never replaces the row
checkbox/comment values.

For every applicable email, `Delivery QA - Email` is also the Content QA coverage
boundary. Compare its exact preflight row labels with the final submitted row
labels. Every source-visible content row and every blocked render/proof row must
be present. Report creation must not start while the difference is non-empty.
