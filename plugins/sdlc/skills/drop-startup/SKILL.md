---
name: drop-startup
description: Remove a legacy startup/bootstrapping class in a workspace repository (e.g. Startup.cs + Program.cs pair, or a hand-rolled bootstrapper) and replace it with a modern host builder entry point, preserving every service registration and the exact middleware/pipeline order. Use when collapsing Startup into Program.cs or adopting the standard host builder.
argument-hint: "[startup file] [reference repo]"
---

# Drop a Legacy Startup Class

Replace a legacy startup/bootstrapping class with a modern host builder entry
point. The full registration set and pipeline order must be preserved exactly.
Follow every rule in `[workspace]\CLAUDE.md` (approval vocabulary, one
step at a time, human runs builds and tests).

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
- The startup/bootstrap file(s) to remove.
- The designated reference repository or framework pattern to follow.

Ask for any that are missing before Step 1.

## Step 1 — Registration inventory
Read the current startup file(s) in full. Produce an ordered inventory of every
service registration, middleware, and pipeline item — order matters and must be
reproduced.

Services (registration order):

| # | Service / option | Lifetime | Notes |
|---|---|---|---|

Pipeline / middleware (execution order):

| # | Middleware / pipeline item | Notes (conditions, environment gating) |
|---|---|---|

Also capture: configuration sources, environment-specific branches, endpoint
mapping, and any ordering that is load-bearing.

## Step 2 — Read the reference
Read the designated reference entry point (or framework docs). State the exact
file used as the pattern.

## Step 3 — Proposed new entry point
Describe the new entry point section by section. Include an explicit
before/after comparison of middleware order so nothing is silently reordered.

| # | Before (legacy) | After (new) | Same position? |
|---|---|---|---|

Wait for approval before writing.

## Step 4 — Write the new entry point
Write the new entry point. Confirm, item by item against the Step 1 inventory,
that every service and pipeline item is accounted for. Report any item
intentionally dropped and why (must already be approved).

## Step 5 — Build
Human runs the build. Interpret the output. Fix any error (propose → approve →
apply). Do not proceed until the build is clean.

## Step 6 — Delete legacy files
Only after a clean build. List the files to be deleted explicitly. Wait for
approval. Delete.

## Step 7 — Build again
Human runs the build again. Interpret the output. Fix any error.

## Step 8 — Integration tests
Human runs the integration tests. Interpret the output. For each failure:
root cause, proposed fix, approval, apply, re-run.

## Completion
Run the `sdlc:context-update` skill to refresh `architecture.md`,
`architecture-graph.md`, `build-system.md` (if entry/run changed), and
`change-journal.md`.
