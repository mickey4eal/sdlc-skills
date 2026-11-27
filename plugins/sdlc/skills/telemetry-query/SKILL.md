---
name: telemetry-query
description: Read-only investigation of telemetry and logs for a deployed environment of the workspace's services — APM requests, dependencies, exceptions and traces (e.g. Application Insights), following one operation across services, and unsampled pre-aggregated metrics — producing an evidence timeline for the calling skill. Use when a bug, incident, performance question or verification depends on what actually happened in an environment, or when asked to check the logs or App Insights. Called by sdlc:bug-fix, sdlc:incident-triage and sdlc:perf-investigation.
argument-hint: "[environment] [question or operation id]"
---

# Telemetry Query

Find out what actually happened in a deployed environment from its telemetry,
and hand the evidence back to the skill that needed it. Read-only: no changes
to telemetry settings, code, configuration or data. Follow every rule in
`[workspace]\CLAUDE.md`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Working agreement" and "Hosting and observability" sections of
   `[workspace]\workspace-settings.md` — in particular "Telemetry access" (the
   resource per environment, query route, sign-in, sampling, retention).
3. Read the affected repos' `bootstrap.md` and `known-issues.md`.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it exists.

## Inputs required from the human
- Environment.
- The question to answer, or the hypothesis to confirm or rule out.
- Time window (when it happened; state the time zone given).
- Any identifiers: operation / correlation id, route, entity id, user, error
  text.

Ask for any that are missing before Step 1.

## Step 1 — Frame the question

| Item | Detail |
|---|---|
| Environment | |
| Telemetry resource (from the settings) | |
| Service / role names in scope | |
| Window (UTC) | |
| Identifiers | |
| Question / hypothesis | |

Convert every time to UTC before querying, and say so.

## Step 2 — Access check
- If the working agreement says Claude runs telemetry queries: confirm the
  session is signed in to the right tenant and subscription with the CLI the
  settings name (e.g. `az account show`). On an expired sign-in, MFA or consent
  error, stop and ask the human to sign in again; never change authentication,
  roles or settings to get access.
- If the human runs queries, or Claude has no access: give each query in full
  and where to run it (e.g. the portal's Logs blade for the named resource), and
  record what the human pastes back.

## Step 3 — Coverage check
Before reading any result as proof, establish what the telemetry can show:

| Check | How | Result |
|---|---|---|
| Sampling | `itemCount` > 1 on rows, or the settings' sampling rate | |
| Retention | Window inside the resource's retention? | |
| Emits traces / exceptions? | Count by item type for the role in the window | |
| Role names | Distinct `cloud_RoleName` matching the services in scope | |

Rule: **zero rows does not mean it did not happen.** If the operation may have
been sampled out, use unsampled pre-aggregated metrics (e.g.
`az monitor metrics list` on the resource: request / dependency count and
duration split by role, result code or success) to establish that it happened,
and say which conclusions rest on sampled data.

## Step 4 — Query, narrow to wide
State each query's purpose before running it. Every query has a time filter
and a row limit. Start narrow, then widen only as the evidence requires:

1. Failed requests and dependencies for the roles in scope, in the window.
2. Exceptions grouped by problem, with a sample message each.
3. One failing operation end to end: requests, dependencies, exceptions and
   traces sharing its `operation_Id`, across every service it touched.
4. Comparison with a successful operation of the same kind, where the
   difference matters.

Starting points (Application Insights KQL; adapt names to the settings):

```kql
// 1. Failures by role and result code
union requests, dependencies
| where timestamp between (datetime(<start>) .. datetime(<end>))
| where cloud_RoleName in (<roles>) and success == false
| summarize count = sum(itemCount) by itemType, cloud_RoleName, name, resultCode
| top 50 by count
```

```kql
// 2. Exceptions by problem
exceptions
| where timestamp between (datetime(<start>) .. datetime(<end>))
| where cloud_RoleName in (<roles>)
| summarize count = sum(itemCount), sample = any(outerMessage), op = any(operation_Id) by problemId, cloud_RoleName
| top 25 by count
```

```kql
// 3. One operation end to end
union requests, dependencies, exceptions, traces
| where timestamp between (datetime(<start>) .. datetime(<end>))
| where operation_Id == "<operation id>"
| project timestamp, itemType, cloud_RoleName, name, target, resultCode, success, duration, message, outerMessage
| order by timestamp asc
| take 200
```

```kql
// Performance baseline (for sdlc:perf-investigation)
requests
| where timestamp between (datetime(<start>) .. datetime(<end>))
| where cloud_RoleName == "<role>" and name == "<operation>"
| summarize count = sum(itemCount), p50 = percentile(duration, 50), p95 = percentile(duration, 95), p99 = percentile(duration, 99) by bin(timestamp, 1h)
```

Record every query run:

| # | Purpose | Tables | Window (UTC) | Rows | Finding |
|---|---|---|---|---|---|

Explain a result code before relying on it (e.g. a SQL dependency result code
of `-2` is a command timeout; HTTP `0` is often a client-side abort).

## Step 5 — Evidence summary

| Time (UTC) | Service | Item | Result | Duration | Message |
|---|---|---|---|---|---|

Then state:
- What the evidence proves.
- What it does not prove (sampled out, not instrumented, outside retention).
- Gaps, and the next query or other source that would close them.

Summarise personal data (names, emails, client details) rather than pasting it.

## Step 6 — Record and hand back
On approval, write the framing, query log (with each full query) and evidence
summary to `[workspace]\tasks\[ticket-id]\telemetry-[date].md`. Then return
to the calling skill's step with the evidence summary.

## Rules
- Read-only. Never change sampling, diagnostic settings, alerts, retention or
  role assignments.
- Times in UTC throughout.
- Bounded queries only: time filter plus `take` / `top` / `summarize`.
- A conclusion drawn from sampled data says so.

## Completion summary
- The question and the answer the evidence supports.
- Key operation ids and the timeline.
- Coverage limits that affect the conclusion.
- Where the query log was recorded.
