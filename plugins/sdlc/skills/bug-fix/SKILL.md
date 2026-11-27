---
name: bug-fix
description: Diagnose and fix a defect in a workspace repository with the minimum change, protected by a regression test that fails first. Use when observed behaviour diverges from expected behaviour — a reported defect, bug ticket, production failure, stack trace, or reproducible error — and the cause must be found and corrected. Prefer this over engineering:debug for work in this workspace. Not for behaviour changes or new functionality dressed as fixes.
argument-hint: "[ticket-id] [defect summary]"
---

# Fix a Bug

Diagnose and fix a defect with the minimum change necessary, protected by a
regression test. Follow every rule in `[workspace]\CLAUDE.md`
(approval vocabulary, one step at a time, human runs builds and tests).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md`.
3. Run the `sdlc:context-first` skill before touching application code.
4. If a ticket id was given (`$ARGUMENTS`), read
   `[workspace]\tasks\[ticket-id]\` if it exists.

Not for behaviour changes dressed as fixes. If the "fix" changes a contract or
adds functionality, stop and use the `sdlc:new-feature` or `sdlc:migrate-endpoint` skill.

## Inputs required from the human
- The defect: observed behaviour vs expected behaviour.
- Reproduction steps and the environment where it occurs.
- Any error message, stack trace, or log excerpt.
- Affected repo/area, if known.

Ask for any that are missing before Step 1.

## Step 1 — Reproduce
If the defect was seen in a deployed environment, first run the
`sdlc:telemetry-query` skill to gather the failing operation's evidence
(requests, dependencies, exceptions, traces). Its timeline shapes the repro
below and feeds the Step 2 call path.

Establish a reliable, local reproduction before proposing any cause. State the
exact steps and the observed failure.

| Repro step | Expected | Actual |
|---|---|---|

If a local reproduction needs a deployed environment or other costly setup
(e.g. role switching), but the defect is clear from the code, propose a failing
regression test as the repro instead. Describe it and wait for approval. That
test is then the one written in Step 5, and the environment check moves to
Step 7. Otherwise, if the defect cannot be reproduced, stop. Report what was
tried and the information still needed. Do not guess at a fix against an
unreproduced defect.

## Step 2 — Locate and isolate
Narrow the failure to the responsible code path. Read the code first; do not
hypothesise from names. Before stating what a caller sends or what a path does,
trace every branch that builds it (each mode, option and default), not only the
first one read. Record the call path from entry point to failure.

| Layer / file | Role in the path | Observation |
|---|---|---|

## Step 3 — Root cause
State the actual mechanism of the defect, not the symptom. One row.

| Symptom | Mechanism | Root cause |
|---|---|---|

Distinguish root cause from proximate trigger. If several causes are plausible,
list them and state how each was confirmed or ruled out. Wait for approval of
the root cause before designing the fix.

## Step 4 — Fix design (minimal blast radius)
Propose the smallest change that corrects the root cause. State the change, the
files touched, and why nothing wider is needed. Call out any behaviour that
callers currently depend on and confirm the fix preserves it. For filter, flag
or selection changes, add a behaviour table of inputs (all, none, a subset,
defaults) against current and expected results, checked against the acceptance
criteria. Wait for approval.

| Change | File | Why minimal | Contract preserved? |
|---|---|---|---|

## Step 5 — Regression test first
Write a test that reproduces the defect and therefore **fails** against the
current code. Follow the project's existing test conventions; state the
reference test used. Describe the test before writing it. Wait for approval,
then write.

Human runs the new test and confirms it fails for the expected reason. A test
that passes before the fix does not protect against this defect — revise it.

## Step 6 — Apply the fix
Apply the approved change from Step 4. Human runs the build (build gate).
Interpret the output; fix any failure (propose → approve → apply) before
continuing.

## Step 7 — Verify
- Human re-runs the Step 1 repro: the defect no longer occurs. Where Step 1
  used a test as the repro, the human runs the agreed environment check
  instead.
- Human runs the Step 5 regression test: it now passes.
- Human runs the surrounding test suite: no new regressions.
- Once deployed, where Step 1 used telemetry: re-run the relevant
  `sdlc:telemetry-query` failure query for a window after the deploy to confirm
  the error no longer occurs (allowing for sampling).
- Search for the same faulty pattern elsewhere. Report the search terms and
  results; record any other occurrences under "Observations for later" (fix them
  only if explicitly approved as in-scope).

| Check | Result |
|---|---|

## Step 8 — Completion summary
- Root cause (from Step 3).
- The fix and files changed.
- The regression test added.
- Build and test status as reported by the human.
- Other occurrences found and where recorded.

Then run the `sdlc:context-update` skill to refresh
`known-issues.md`, `change-journal.md`, and any affected context files. Before a
deploy or the human's PR, suggest the `sdlc:code-review` skill if the change has
not been reviewed.
