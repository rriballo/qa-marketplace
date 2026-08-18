---
name: qa-journey-campaign
description: Audit the properties, graph, activities, waits, events, conditions, scripts, splits, transitions, hygiene, messages, and technical dependencies of a supported Adobe Journey Optimizer journey or campaign. Read-only.
---

# Audit journey or campaign

Read `shared/mcp-capability-map.md` and discover capabilities first. The
documented CX Coworker Gateway baseline supports campaign metadata, targeting,
schedule, channel configuration, and available content metadata, but not a
complete Journey graph. Use `ajo_campaign_list` to resolve campaigns and
`ajo_campaign_get` to re-read each campaign. Confirm whether a custom server
exposes `read_journey_graph` and the requested graph depth; otherwise mark graph,
activity, script, and logic checks `BLOCKED`. Re-read the object and each
referenced resource before making detailed claims.

When the user supplies a complete or partial journey/campaign payload, treat its
fields as inspectable evidence even if a custom graph tool is absent. Prefer a
newer authoritative runtime re-read when available and record conflicts. Follow
`shared/qa-execution-contract.md` for status order and objective checks.

Check label, description, targeting, schedule, channel, and available content
metadata first. Check entry, re-entry, timezone, profile timezone, dates,
frequency, timeout/error settings, lifecycle state, activity labels, selected
audience/event, identity type, waits, quiet hours, date/time conditions, scripts,
percentage splits, variants, transitions, consent/blacklist hygiene, selected
messages, dead ends, unexpected safety conditions, and end-to-end business logic.

For scripts, check compilation and referenced fields only if the MCP exposes those
operations; otherwise report the validation as `BLOCKED`. Capture IDs, version,
status, timestamps, dependencies, and graph evidence. Do not claim the journey is
deliverable merely because its structure is readable.

For reporting, map each result to the exact `Journey QA` or applicable Campaign
QA row returned by `inspectConfluenceQaTemplate`. Produce one QA1 status and
evidence-backed comment per applicable row. Missing graph access makes the
affected rows `BLOCKED`; it does not permit omitting them. Never write QA2.

Absent optional structures are row-level `NA` only when the graph proves their
absence. Examples include no read-audience activity, wait, percentage split, or
variants. Brief-dependent correctness remains `BLOCKED` when the brief is
missing. Empty descriptions, fixed test values, wrong required identity,
unlabelled required transitions, visible missing hygiene, and unresolved message
references are objective failures when complete evidence exposes them.
