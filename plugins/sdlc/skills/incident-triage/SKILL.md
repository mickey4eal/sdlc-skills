---
name: incident-triage
description: Triage a production or higher-environment issue in the workspace's services — gather evidence (errors, logs, telemetry, error tracking, message dead-letters, recent deployments), assess severity and blast radius, find the most likely cause, and decide between mitigate, sdlc:hotfix, sdlc:bug-fix or monitor. Read-only on systems. Use when something is broken or degraded in any deployed environment, or an alert or user report needs assessment.
argument-hint: "[environment] [symptom]"
---

# Incident Triage

Understand an incident fast enough to make the right call. Read-only: no
changes to code, configuration, data or infrastructure in any environment. The
human gathers anything Claude cannot reach. Follow every rule in
`[workspace]\CLAUDE.md`. Best run in `Mode: analysis`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Environments and releases" and "Hosting and observability"
   sections of `[workspace]\workspace-settings.md`, and the hosting
   reference it points to.
3. Read the suspected repos' `bootstrap.md` and `known-issues.md`.

## Inputs required from the human
- Environment and symptom (what users see, error text, alert).
- When it started and whether anything was deployed around then.
- Evidence available: logs, telemetry queries, error-tracking events,
  dead-letter counts, screenshots.

Ask for any that are missing before Step 1.

## Step 1 — Frame the incident

| Item | Detail |
|---|---|
| Environment | |
| Symptom | |
| Started | |
| Users / processes affected | |
| Recent changes (deploys, config, infra, data) | |
| Severity (proposed) | |

Severity: `SEV1` service down / data loss, `SEV2` major function broken, no
workaround, `SEV3` degraded or workaround exists, `SEV4` cosmetic / minor.

## Step 2 — Evidence
Collect and record each piece of evidence and what it tells us. Ask the human
for specific queries or screenshots where Claude has no access — give the exact
query or place to look.

| Source | Evidence | What it tells us |
|---|---|---|

Typical sources, using the tools named in the settings: application and
function host logs, telemetry exceptions and dependency calls, error tracking,
message dead-letter queues, database errors and blocking, pipeline history,
secret store / identity / role-assignment changes.

For telemetry and logs, run the `sdlc:telemetry-query` skill (coverage and
sampling check, failing-operation timeline) and record its evidence summary
here.

## Step 3 — Hypotheses

| Hypothesis | Supporting evidence | Contradicting evidence | How to confirm |
|---|---|---|---|

Check `known-issues.md`, recent `change-journal.md` entries and the settings'
known incident hazards first — many incidents are a known hazard.

## Step 4 — Decision
Recommend one, with reasons:
- `mitigate` — immediate operational action (restart, re-grant, config
  rollback, resubmit dead-letters) that the human performs.
- `sdlc:hotfix` — code fix needed before the next release.
- `sdlc:bug-fix` — fix in the normal flow; impact tolerable.
- `monitor` — not confirmed / self-resolving; state what to watch.

## Step 5 — Record
Write the incident summary (timeline, evidence, cause, decision, follow-ups) to
`[workspace]\tasks\[ticket-id]\incident-[date].md` on approval.

## Completion summary
- Severity and confirmed or most likely cause.
- Decision and owner of the next action.
- Follow-ups, including any known-issue to record via `sdlc:context-update`.
