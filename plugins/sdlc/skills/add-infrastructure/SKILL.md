---
name: add-infrastructure
description: Add a cross-cutting infrastructure extension to a workspace repository — logging, auth, caching, health checks, telemetry, a message-bus client, a resilience policy — by following a reference implementation, then wiring it into the entry point and configuration. Use when introducing a concern wired at the application entry point that other code depends on indirectly. Not for business features (use sdlc:new-feature).
argument-hint: "[concern] [reference implementation]"
---

# Add Infrastructure

Add a new infrastructure extension or cross-cutting concern to a project.
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
- The concern to add.
- The designated reference implementation (reference repository or existing
  infrastructure in this repo) to follow.

Ask for any that are missing before Step 1.

## Step 1 — Read the reference
Read the reference implementation first. State the exact file and describe the
pattern it uses (extension method shape, options binding, lifetime,
naming/folder placement).

## Step 2 — Identify project-specific adaptations
Document what must differ for this project before implementing — config keys,
service names, environment gating, dependency versions, namespace. One list,
wait for approval of the adaptation set.

| Aspect | Reference | This project |
|---|---|---|

## Step 3 — Create the extension file
Place it in the correct infrastructure folder for this project. Describe the
file before writing. Wait for approval, then write.

## Step 4 — Wire into the entry point
State the exact registrations to add to the application entry point
(registration call and, if applicable, pipeline placement) and where in the
existing order they go. Wait for approval, then write.

## Step 5 — Configuration
Add any new configuration keys required, with safe defaults, in the correct
settings file(s). Do not put secrets in source. Describe before writing. Wait
for approval, then write.

## Step 6 — Build
Human runs the build. Interpret the output. Fix any error (propose → approve →
apply).

## Completion
Run the `sdlc:context-update` skill to refresh `architecture.md`, `dependency-map.md`
(if a new external dependency or integration), `conventions.md` (if a reusable
pattern), and `change-journal.md`.
