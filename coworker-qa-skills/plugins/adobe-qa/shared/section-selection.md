# Conditional QA Section Selection

Select sections only after resolving and re-reading the Adobe object and every
referenced dependency. Do not infer applicability from `qa_type` alone.

| Evidence found | Include |
|---|---|
| Journey with an audience or segment reference | Audience, Journey/Campaign |
| Journey with no audience reference | Journey/Campaign; populate existing Audience QA rows as `NA` with reason |
| Email action or email delivery reference | Email Delivery; applicable Proof/Preview/Personalization |
| SMS action or SMS delivery reference | SMS Delivery; applicable Proof/Preview/Personalization |
| Campaign with an audience or segment reference | Audience, Journey/Campaign |
| Campaign with a delivery/channel reference | Delivery section for each channel |
| Delivery-only object | Delivery section for each channel; populate fixed Audience and Journey/Campaign rows as `NA` where represented |

An action is applicable when present in the Adobe object, linked dependency
metadata, or brief and tied to the object by an identifier. A brief-only
expectation does not make an absent object applicable; report the mismatch.

Create an internal record for every candidate section with `section`,
`included`, `reason`, `source/evidence`, and resolved identifiers. Do not add
this record as a new Confluence section. Use it to populate matching regions
already present in the duplicated template. Omitted sections must state why they
are genuinely non-applicable. In the fixed Confluence template, do not remove
sections. When a represented dependency is proven absent, populate its existing
checklist rows as `NA` with the structural reason. Included sections run all
relevant checks, even when the MCP cannot support them.

Unavailable capabilities are `BLOCKED`, never `N/A`, with the missing tool and
attempted scope. Reserve `N/A` for a check that is irrelevant to the selected
configuration.
