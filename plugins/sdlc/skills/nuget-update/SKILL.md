---
name: nuget-update
description: Safely update package references in a workspace repository (NuGet primarily, npm by the same discipline) — audit current versions and drift, assess breaking changes against call sites, update one package per build gate, sweep deprecated APIs. Use when bumping package versions, moving to latest stable, or consolidating versions across projects.
argument-hint: "[package(s)] [target version]"
---

# Update Package References

Safely update package references. Follow every rule in
`[workspace]\CLAUDE.md` (approval vocabulary, one step at a time, human
runs builds and tests).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md`.
3. Run the `sdlc:context-first` skill before touching application code.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- Packages and target versions (or "latest stable").
- Scope: whole solution, or named projects only.

Ask for any that are missing before Step 1.

## Step 1 — Audit current versions
Produce a table of the current version of each in-scope package across all
project files. Flag version drift (same package, different versions across
projects).

| Package | Project | Current version | Target version |
|---|---|---|---|

## Step 2 — Breaking-change assessment
For each package being updated, identify breaking changes between current and
target version (release notes / changelog). Flag the ones that affect this
codebase, citing the call sites.

| Package | Current → Target | Breaking change | Affects this code? | Where |
|---|---|---|---|---|

## Step 3 — Update plan
Consolidate into the plan to approve. Wait for approval before changing any
project file.

| Package | Current | Target | Breaking-change assessment | Action |
|---|---|---|---|---|

## Step 4 — Apply, one package at a time
For each package, in order:
1. State the project-file change. Wait for approval.
2. Apply.
3. Human runs the build. Interpret the output. Fix before proceeding.

Do not batch updates. One package, one build gate.

## Step 5 — Deprecated-API sweep
After updates, search for deprecated/removed APIs introduced by the version
changes (compiler warnings + known removals from Step 2). Propose fixes, wait
for approval, apply.

## Step 6 — Tests
Human runs the full test suite. Interpret the output. For each regression:
root cause, proposed fix, approval, apply, re-run.

## Step 7 — Completion summary
- Packages updated (from → to).
- Breaking changes handled.
- Build and test status as reported by the human.
- Deferred items under "Observations for later".

Then run the `sdlc:context-update` skill to refresh `dependency-map.md` and
`change-journal.md`.
