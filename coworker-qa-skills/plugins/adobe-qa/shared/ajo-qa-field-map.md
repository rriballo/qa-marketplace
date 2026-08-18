# AJO QA Evidence Field Map

Use this map to translate a supplied or runtime Adobe object into template-row
evidence. Actual response schemas remain authoritative; cite the returned path.

## Overview

| Template value | Typical journey evidence | Typical campaign evidence |
|---|---|---|
| Configuration responsible | `metadata.createdBy`, `metadata.lastModifiedBy` | creator/owner/last-modifier fields |
| Journey/Campaign label | `name` | `name` or `label` |
| Journey/Campaign URL | runtime URL; otherwise object ID with `URL not exposed` | runtime URL; otherwise object ID with `URL not exposed` |
| Start/end date | `startDate`, `endDate` | schedule start/end fields |
| Type | `isBatch`, entry node type, batch/schedule definition | scheduled, API-triggered, or recurring campaign fields |
| Time zone | `timezone.zoneId`, `timezone.useProfileTimezone` | schedule/channel timezone fields |
| Market/country/language | brief first; otherwise explicitly labelled derivation from conditions/content | brief first; otherwise explicitly labelled targeting/content derivation |
| Audience URL | resolved linked audience URL/ID; `N/A` only when no relationship exists | resolved target audience URL/ID |

Never present a derivation as a briefed fact.

## Journey QA

| Row | Typical evidence |
|---|---|
| Label / Description | root `name`, `description` |
| Entrance | `isBatch`, entry node type/category, `reentrancePolicy`, `noReentrancePeriodInSecs` |
| Time zone / Profile time zone | `timezone.zoneId`, `timezone.useProfileTimezone` |
| Start date & End date | `startDate`, `endDate`, schedule fields |
| Timeout/error configuration | root timeout fields and each activity's fallback/error fields |
| Activities labels | `ui.nodes[*].label`, `ui.nodes[*].data.label`, names and spelling |
| Orchestration | entry and orchestration node types |
| Correct audience | read-audience or audience-qualification IDs and re-read audience |
| Wait activities | wait node duration/date/timezone fields; `NA` only after complete graph inspection |
| Events | event node UID, event ID, category, behavior, namespace, payload fields |
| Activities / identity | root and activity identity namespace, normally `HashedKOCID` |
| Conditions | condition expressions, branch IDs, labels, date/time boundaries |
| Percentage split / Variants | split percentages and variant branches; `NA` when absent |
| Transitions | `ui.edges[*]` source, target, label, type, and graph reachability |
| Hygiene rules | consent/blacklist/suppression conditions visible in the complete graph |
| Message | action UID, message/content ID, channel, surface, and re-read dependency |
| Journey logic | complete node/edge traversal compared with the brief; block without expected narrative |
| Custom checks | literal recipients, test values, safety branches, dead ends, expired dates, risky deployed settings |
| Delete | temporary safety conditions identified by the brief or naming; block when intent is unknown |
| Tech details | IDs, version, state, validation, timestamps, merge policy, namespace, node/edge/action IDs |

## Delivery QA - Email

| Row | Typical evidence |
|---|---|
| Label / Description | action/delivery `label`, `description` |
| Channel | `channel`, category/type |
| Channel Surface | `surfaceId` followed by channel-configuration re-read |
| Settings | tracking, optimization, frequency, fallback/error fields |
| Email parameters | address/channel override and profile-field expression |
| From, subject, pre-header | resolved message/content or channel configuration |
| Designer/content rows | message HTML/content, assets, links, metadata, rendered proof |
| Personalization | expressions plus validation and test-profile preview evidence |
| Preview | actual desktop/mobile render evidence |

A fixed literal recipient on a deployed or production-intended action is a
failure unless explicitly approved. Content/designer rows are `BLOCKED`, not
`PASS`, when only action metadata is available.

## Audience QA

Resolve the exact referenced audience before checking label, description,
template/type, evaluation, status, included audiences, attributes, events,
include/exclude logic, all/any logic, operators, containers, and counts. If the
complete graph proves no audience relationship exists, all Audience QA rows are
`NA`. If a reference exists but cannot be resolved, all unavailable checks are
`BLOCKED`.

## Campaign QA

Use `ajo_campaign_get` and linked dependencies for campaign identity, lifecycle,
schedule, targeting/audience, channel, surface, content reference, tracking, and
technical metadata. Map only to an exact Campaign QA section returned by template
preflight. Never write campaign-core results into Journey QA rows. If no Campaign
QA section is returned, report the template coverage gap and do not claim a
complete campaign QA.
