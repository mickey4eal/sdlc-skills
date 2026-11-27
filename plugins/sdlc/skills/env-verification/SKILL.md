---
name: env-verification
description: Verify a delivered ticket in a deployed environment (e.g. SIT, UAT) for a workspace repository — re-check the ticket, map each acceptance criterion to the merged code, list prerequisite checks, write a manual test plan with expected results, and record the results the human reports in the task's manual-testing.md. The human runs every case. Use when a ticket's work is merged or deployed and needs testing or sign-off in an environment, or when asked for a manual or SIT test plan.
argument-hint: "[ticket-id] [environment]"
---

# Environment Verification

Prove a delivered ticket meets its acceptance criteria in a deployed
environment. Claude plans, diagnoses and records; the human runs every case and
reports the outcome. Follow every rule in `[workspace]\CLAUDE.md`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\tasks\[ticket-id]\` and the `bootstrap.md` and
   `testing-strategy.md` of each repo involved.
3. Re-fetch the ticket as in `sdlc:ticket-analysis` Step 0 and report any change
   since the task folder's snapshot; test against the current ticket.
4. Use an existing `tasks\*\manual-testing.md` as the layout reference and
   state which.

## Inputs required from the human
- Ticket id and target environment.
- What has been deployed there (PRs, release branch) if not in the task folder.

Ask for any that are missing before Step 1.

## Step 1 — AC-to-code check
For each acceptance criterion, find the merged code that delivers it and the
automated tests that already cover it. Flag any criterion with no delivering
code or that the code meets differently from the ticket's wording.

| AC | Delivered by (file / PR) | Automated coverage | Gap or difference |
|---|---|---|---|

Wait for approval before Step 2.

## Step 2 — Prerequisites
Checks that must pass before any case runs: the right build and database
changes are in the environment, endpoints or UI are present, test data and
users exist. Give the query or check for each.

| # | Check | Result |
|---|---|---|

## Step 3 — Test plan
One row per case, each tied to an AC, covering the happy path, boundaries,
idempotency, wrong input, missing data and permissions. Give the exact step and
the expected result (status code, rows, side effects such as messages, audit
or search updates). Note cases that depend on data or users that may not exist.
A case outside the ticket's AC (a control, cache path or similar) has
`optional extra` and its purpose in the AC column; optional extras are not
counted in pass/fail and are left out of the ticket comment.

| # | AC | Step | Expected | Result |
|---|---|---|---|---|

Write the plan to `tasks\[ticket-id]\manual-testing.md` after approval. Do not
offer to run the cases or ask for tokens to run them.

## Step 4 — Record results
As the human reports outcomes, fill the Result column. For an unexpected
result, diagnose from what the human pastes and state whether it is a defect
(→ `sdlc:bug-fix`), a test-data or environment issue, or a wrong expectation;
propose the next check. Update the summary line at the top with the date and
pass/fail counts for AC cases, with any optional extras run listed separately.

When the human wants a ticket comment, draft the short form: one ✅/❌ line per
AC area, the key cause of any failure, then short questions for the team. No
tables; the detail stays in `manual-testing.md`.

## Completion summary
- AC cases passed / failed / not run, and why any were not run; optional
  extras run.
- Defects raised and the skill handling each.
- AC gaps or differences from Step 1.

Then record durable findings (environment quirks, test users, seed data) under
"Observations for later" for the next `sdlc:context-update` run.
