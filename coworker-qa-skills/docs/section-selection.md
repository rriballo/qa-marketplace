# Conditional QA Section Selection

Resolve and re-read the Adobe object and referenced dependencies before choosing
report sections. The `qa_type` input is not sufficient evidence of scope.

| Resolved evidence | Sections |
|---|---|
| Journey with an audience/segment reference | Audience, Journey/Campaign |
| Journey without an audience reference | Journey/Campaign; omit Audience with reason |
| Email action/delivery | Email Delivery, plus applicable Proof/Preview/Personalization |
| SMS action/delivery | SMS Delivery, plus applicable Proof/Preview/Personalization |
| Campaign with audience/segment | Audience, Journey/Campaign |
| Campaign with delivery/channel | Delivery section for each channel |
| Delivery-only object | Delivery section(s); omit Audience and Journey/Campaign |

Record every candidate section as `included` or `omitted`, with a reason,
source/evidence, and resolved identifiers. Omit only genuinely non-applicable
sections. Any unsupported check within an included section is `BLOCKED`, not
`N/A`; reserve `N/A` for a check irrelevant to that configuration.
