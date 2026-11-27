---
name: perf-investigation
description: Measurement-first performance investigation in a workspace repository — establish a baseline, locate where time goes (database query plans and indexes, N+1 data access, missing async, chatty HTTP or messaging calls, search queries, frontend re-renders), propose the smallest change that fixes the bottleneck, and prove it against the baseline. Use when an endpoint, function, query or page is slow or resource-heavy.
argument-hint: "[endpoint | query | page]"
---

# Performance Investigation

Find and fix a real bottleneck, proven by numbers. Follow every rule in
`[workspace]\CLAUDE.md` (approval vocabulary, one step at a time, human
runs builds, tests and measurements).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Performance tooling" section of
   `[workspace]\workspace-settings.md`.
3. Read the repo's `bootstrap.md`, `architecture.md` and `known-issues.md`.
4. Run the `sdlc:context-first` skill before touching application code.
5. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- What is slow, where (environment), and how slow (observed vs acceptable).
- How to reproduce: request, data volume, user.
- Available telemetry (from the settings' tooling, or what else exists).

Ask for any that are missing before Step 1.

## Step 1 — Baseline
Agree how to measure and record the baseline. No change is evaluated without it.

| Scenario | Measure (p50 / p95 / duration / RU / rows) | Baseline | Target |
|---|---|---|---|

Give the human the exact measurement to take with the settings' tooling (for
example a telemetry query, database statistics plus the actual execution plan,
or a browser performance trace). For telemetry baselines (percentiles,
dependency durations), run the `sdlc:telemetry-query` skill; where telemetry is
sampled, take counts from unsampled metrics.

## Step 2 — Locate the time
Trace the path from entry point to data store; read the code. Attribute time to
segments using the evidence.

| Segment | File / object | Time or cost | Evidence |
|---|---|---|---|

Look for: per-row queries in loops (N+1), missing or non-covering indexes, scans
on large tables, implicit conversions, parameter sniffing, unbounded result
sets, sync-over-async, serial calls that could be concurrent, repeated
deserialisation, over-fetching columns, unnecessary frontend re-renders or
unmemoised selectors.

## Step 3 — Root cause and fix design

| Bottleneck | Cause | Proposed change | Expected gain | Risk |
|---|---|---|---|---|

Propose the smallest change. Index or schema changes go through the workspace's
database-change skill or process, if it has one (otherwise propose the
migration explicitly, with its deployment order and rollback); contract changes
run `sdlc:impact-analysis` first. Wait for approval.

## Step 4 — Apply one change at a time
Describe, wait for approval, apply. Human builds and re-measures after each
change. Do not stack changes before measuring.

| Change | Before | After | Kept? |
|---|---|---|---|

## Step 5 — Regression guard
Where practical, add a test or check that would catch the regression (query
count assertion, result-size limit, benchmark). Describe before writing.

## Completion summary
- Baseline vs final numbers.
- Changes kept and discarded.
- Remaining bottlenecks recorded under "Observations for later".

Then run the `sdlc:context-update` skill (`known-issues.md`, `change-journal.md`).
