---
name: qa-delivery
description: Audit an Adobe Journey Optimizer delivery or message configuration and content against a brief, channel guidelines, personalization requirements, tracking, accessibility, and preview evidence. Read-only.
---

# Audit delivery

Call capability discovery first and use only tools actually exposed. Re-read the
delivery/message before reporting fields. The documented CX Coworker Gateway
does not expose message HTML, subject lines, personalization tokens, offer
content, or rendered proofs. Mark those checks `BLOCKED` unless a custom AJO MCP
provides them. Check label, description, category,
surface, tracking, profile email fields, sender, subject, preheader, fonts,
margins, images and labels, links and UTM parameters, social URLs, mirror page,
unsubscribe, personalization syntax/fallbacks, and desktop/mobile preview when
the connection supports it.

Missing image label is a blocker in the supplied QA standard. Missing title or
alt text is a finding unless the brief makes it a blocker. Do not claim a link is
working or a preview is correct without evidence. Never send a live proof.
