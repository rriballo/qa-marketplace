# Audience QA

Compare the configured AEP audience with the brief/SRS and return atomic results
using `shared/qa-result-contract.md`.

When the resolved journey/campaign proves no audience relationship exists,
return `NA` with that reason for every Audience QA row. An unresolved referenced
audience is `BLOCKED`, not `NA`.

## Checks

- Audience label follows taxonomy exactly, including market, country, language,
  campaign code, and version.
- Description is populated and describes the intended population.
- Audience definition uses the required profile schema and attributes.
- Inclusion criteria match the brief, including dates, geography, brand, status,
  consent, and eligibility conditions.
- Exclusions match the brief, including blacklist, suppression, prior-contact,
  and control rules.
- Merge policy, evaluation method, status, and refresh cadence are appropriate.
- Identity namespace is correct. For Core/Prospect Profile, require `HashedKOCID`
  unless the brief explicitly specifies another namespace.
- Audience is the exact audience selected by the journey/campaign.
- Audience counts are present and plausible; unexplained zero or extreme counts
  are `FAIL` or `BLOCKED` depending on available requirements.
- Audience URL and technical identifiers are recorded as evidence.

Do not infer population correctness from the label alone. If the audience
definition or counts cannot be inspected, return `BLOCKED`.

When exposed, use `ajo_aep_get_audience` with the exact referenced system ID for
definition, expression, schema, evaluation, governance, and dependency evidence.
Use `ajo_aep_list_audiences` only for paginated discovery. Same-name results do
not prove linkage, and these tools do not prove counts, qualification, identity
graph, consent state, or activation.
