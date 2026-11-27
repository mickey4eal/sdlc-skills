---
name: test-authoring
description: Add or expand tests around existing, working code in a workspace repository — map coverage gaps, plan happy/boundary/edge/error/characterisation cases, follow existing test conventions, author one test per run. Use when closing coverage gaps, writing characterisation tests before a refactor or migration, or covering untested edge and failure paths. Not for a defect's regression test (use sdlc:bug-fix) or tests for a brand-new feature (use sdlc:new-feature). Prefer this over engineering:testing-strategy for workspace repos.
argument-hint: "[code under test] [coverage goal]"
---

# Author Tests for Existing Code

Add or expand tests around existing code — closing coverage gaps, protecting
edge cases, and locking in current behaviour. Follow every rule in
`[workspace]\CLAUDE.md` (approval vocabulary, one step at a time, human
runs builds and tests).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md` and
   `testing-strategy.md`.
3. Run the `sdlc:context-first` skill before touching application code.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

Not for the regression test that accompanies a defect fix — that belongs in the
`sdlc:bug-fix` skill. Not for tests written alongside a brand-new feature — that
belongs in the `sdlc:new-feature` skill.

## Inputs required from the human
- The code under test (files/module/feature).
- The coverage goal (characterise current behaviour, cover a specific path, or
  raise coverage of a named area).
- Whether current behaviour is the source of truth, or a spec is.

Ask for any that are missing before Step 1.

## Step 1 — Map current coverage and gaps
Identify what is already tested and what is not. Read the code under test and the
existing tests for it. Do not write anything yet.

| Behaviour / path | Currently tested? | Gap |
|---|---|---|

## Step 2 — Test plan
Enumerate the cases to add, one row each. Cover happy path, boundaries, edge
cases, and error/failure paths. Wait for approval of the plan before writing.

| Case | Input / precondition | Expected outcome | Type |
|---|---|---|---|

Type values: `happy`, `boundary`, `edge`, `error`, `characterisation`. Where
behaviour is being characterised (locked in as-is rather than judged correct),
mark it so — a characterisation test asserts what the code *does*, not what it
*should* do.

## Step 3 — Identify conventions and reference
State the test framework, fixture style, naming, and arrange/act/assert
conventions in use, and name an existing test file used as the structural
reference. Wait for approval.

## Step 4 — Author tests one at a time
For each approved case, in order:
1. Describe the proposed test. Wait for approval.
2. Write it.
3. Human runs it. Confirm it passes (or, for a case that documents a known gap,
   confirm it fails for the expected reason and record that gap). A pending,
   skipped or inconclusive result — e.g. a scenario with unbound steps — is a
   failure, not a pass.

Do not batch. One case, one run. Do not modify the code under test — if a test
cannot pass without a code change, stop and record it under "Observations for
later"; that is a defect, not a test task.

## Step 5 — Run the suite
Human runs the full suite. Interpret the output, and report any pending,
skipped or inconclusive tests as failures. Ensure no existing test was
destabilised by the additions.

## Step 6 — Completion summary
- Cases added (count / list from Step 2).
- Coverage gaps closed and any deliberately left open.
- Any defects uncovered, recorded under "Observations for later".
- Test status as reported by the human.

Then run the `sdlc:context-update` skill to refresh `change-journal.md` and any
affected context files (e.g. `known-issues.md` if a gap or defect was recorded).
