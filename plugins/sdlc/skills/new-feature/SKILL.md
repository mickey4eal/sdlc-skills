---
name: new-feature
description: Add a new feature to a feature-based (vertical-slice) project in a workspace repository — define operations and contracts, mirror an existing reference feature's folder layout, create registration, handlers, contracts, DI wiring and unit tests. Use when adding a new feature folder with its own endpoints, handlers, contracts and registration. Not for business-agnostic cross-cutting concerns (use sdlc:add-infrastructure).
argument-hint: "[feature-name]"
---

# Add a New Feature

Add a new feature to a feature-based (vertical-slice) project. Follow every
rule in `[workspace]\CLAUDE.md` (approval vocabulary, one step at a
time, human runs builds and tests).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md`.
3. Run the `sdlc:context-first` skill before touching application code, and
   identify an existing feature to use as the structural reference.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- Feature name.
- Operations/endpoints (route, method per operation).
- Request/response contracts.
- Auth requirements per operation.

Ask for any that are missing before Step 1.

## Step 1 — Define the feature
Restate the feature precisely. Produce the definition and wait for approval
before creating anything.

| Operation | Route | Method | Request | Response | Auth |
|---|---|---|---|---|---|

State the existing feature being used as the structural reference (folder
layout, naming, registration style). Wait for approval.

## Step 2 — Create the feature folder structure
Mirror the reference feature's folder layout. Show the proposed folder/file tree
before creating. Wait for approval.

## Step 3 — Endpoint/operation registration file
Create the registration file — mapping only, no business logic. Describe before
writing. Wait for approval.

## Step 4 — Handler / service file(s)
One per operation. Follow the reference handler shape. Describe each before
writing. Wait for approval per file, then write.

## Step 5 — Request and response contracts
Create the contract types for each operation. Match the project's contract
conventions (records vs classes, validation, nullability). Describe before
writing. Wait for approval.

## Step 6 — Feature dependency registration
Create or update the feature's DI registration so handlers and dependencies
resolve. Describe before writing. Wait for approval.

## Step 7 — Wire into top-level aggregators
Update the endpoint aggregator and top-level DI so the feature is mapped and
registered. State the exact registrations and where they go. Wait for approval.

## Step 8 — Unit tests
Create unit test file(s) following the existing test conventions (framework,
fixtures, naming, arrange/act/assert style). Describe before writing. Wait for
approval.

## Step 9 — Build and tests
Human runs the build and tests. Interpret the output. Fix any failure
(propose → approve → apply).

## Step 10 — Run and verify
Human runs the application. Confirm each new operation appears in the API
documentation output with the contracts and auth from Step 1.

| Operation | Route | Method | Request | Response | Auth | Present? |
|---|---|---|---|---|---|---|

## Completion
Run the `sdlc:context-update` skill to refresh `api-surface.md`, `architecture.md`,
`domain-models.md` (if new contracts), and `change-journal.md`.
