---
name: qa-journey-campaign
description: Audit the properties, graph, activities, waits, events, conditions, scripts, splits, transitions, hygiene, messages, and technical dependencies of a supported Adobe Journey Optimizer journey or campaign. Read-only.
---

# Audit journey or campaign

Call capability discovery first. The documented CX Coworker Gateway baseline
supports campaign metadata, targeting, schedule, channel, and content metadata,
but not a complete Journey graph. Confirm whether the connected server supports
the requested object type and graph depth; Adobe Gateway availability is
entitlement and permission dependent. Re-read the object and each referenced
resource before making detailed claims.

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
