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
