---
name: refactor
description: Behaviour-preserving structural refactor in a workspace repository — moving code between projects/folders, reshaping a module, adopting a new structure — with a captured parity baseline, one unit moved per build gate, and a parity check afterwards. Use when reorganising code without changing contracts, schemas or behaviour.
argument-hint: "[objective] [target structure]"
---

# Structural Refactor (Behaviour-Preserving)

Any structural refactor that must preserve existing behaviour. No contract,
schema, or behaviour changes. Follow every rule in
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
- The refactor objective and target structure.
- Any reference repository that already uses the target structure.

Ask for any that are missing before Step 1.

## Step 1 — Capture the baseline (must be preserved)
Document existing behaviour before moving anything. This is the parity contract.

| Endpoint / entry point | Contract / schema | Status codes | Auth | Integration points |
|---|---|---|---|---|

Also record: public types other code depends on, DI registrations, and any
ordering/lifetime that is load-bearing.

## Step 2 — Current-to-target mapping
Map every artefact from its current location to its target location. Wait for
approval before moving anything.

| Artefact | Current location | Target location |
|---|---|---|

## Step 3 — Items to remove
List every artefact to be removed, each with the reason. Wait for approval.

| Artefact | Reason for removal |
|---|---|

## Step 4 — Scaffold the target structure
Create the target folders/projects/registration shells before moving any logic.
Describe before writing. Wait for approval.

## Step 5 — Move logic one unit at a time
Move one feature / module / class at a time. After each move:
- Update references and registrations for that unit only.
- Human runs the build (build gate). Interpret the output. Fix before the next
  unit. Do not batch moves.

## Step 6 — Verify behaviour parity
After all moves, compare the current API surface against the Step 1 baseline.
Any deviation is a defect to fix or an explicitly recorded known deviation —
never silent.

| Baseline item | After refactor | Match | Note |
|---|---|---|---|

## Step 7 — Tests
Human runs the full test suite. Interpret the output. For each regression:
root cause, proposed fix, approval, apply, re-run.

## Step 8 — Update context
Run the `sdlc:context-update` skill to refresh all affected context artefacts.

## Step 9 — Completion summary
- What moved (count / list).
- What was removed.
- What was deferred.
- Known deviations from the Step 1 baseline (should be none; list any with
  reason).
