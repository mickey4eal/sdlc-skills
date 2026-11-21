---
name: migrate-endpoint
description: Migrate an endpoint in a workspace repository from one architectural pattern to another — e.g. MVC controller action to minimal API, into a feature/vertical-slice structure, or between feature modules — preserving route, method, schemas, status codes and auth exactly. Use when the shape of an existing endpoint changes but its contract and behaviour must not.
argument-hint: "[route or controller] [target pattern]"
---

# Migrate an Endpoint

Migrate an endpoint from one architectural pattern to another. Behaviour and
contract are preserved; only the shape changes. Follow every rule in
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
- Source endpoint(s): route, method, controller/handler file.
- Target pattern and the reference endpoint already built that way.

Ask for any that are missing before Step 1.

## Step 1 — Scope statement
Read the source endpoint fully. List every file to be created, moved, or
modified. Capture the contract baseline that must not change.

Files affected:

| File | Action (create / move / modify) | Purpose |
|---|---|---|

Contract baseline (must be identical after migration):

| Route | Method | Request schema | Response schema | Status codes | Auth |
|---|---|---|---|---|---|

Wait for approval before touching anything.

## Step 2 — Target endpoint registration
Read the reference endpoint registration. State the reference file. Describe
the proposed registration file (route, verb, binding, auth, mapping to handler).
Wait for approval, then write.

## Step 3 — Handler(s)
Extract the logic from the source into the target handler shape — preserve
behaviour, do not redesign. One handler per operation. Describe each before
writing. State the reference handler used. Wait for approval, then write.

## Step 4 — Feature dependency registration
Add or update the feature's DI registration so the new handler and its
dependencies resolve. Describe before writing. Wait for approval, then write.

## Step 5 — Aggregators and top-level wiring
Update the endpoint aggregator and any top-level registration so the new
endpoint is mapped and the feature is wired in. State the exact registrations
and where they go. Wait for approval, then write.

## Step 6 — Build
Human runs the build. Interpret the output. Fix any error (propose → approve →
apply). Do not proceed until the build is clean.

## Step 7 — Run and verify
Human runs the application. Confirm the migrated endpoint appears in the API
documentation output (e.g. OpenAPI/Swagger). Produce a comparison table against
the Step 1 baseline.

| Field | Before (source) | After (migrated) | Match |
|---|---|---|---|
| Route | | | |
| Method | | | |
| Request schema | | | |
| Response schema | | | |
| Status codes | | | |
| Auth | | | |

Any mismatch is a defect to fix before the gate, not a deviation to accept.

## Step 8 — Gate
Do not proceed to the next endpoint or feature until this migration is
explicitly approved. The legacy source endpoint is removed only when the human
confirms — list it explicitly and wait for approval (see the `sdlc:refactor` /
`sdlc:remove-package` skills for removal discipline if the source class becomes
empty).

## Completion
Run the `sdlc:context-update` skill to refresh `api-surface.md`, `architecture.md`,
`architecture-graph.md`, and `change-journal.md`.
