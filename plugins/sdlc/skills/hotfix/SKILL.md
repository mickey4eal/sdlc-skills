---
name: hotfix
description: Urgent fix to code already in production for a workspace repository — branch from what is actually deployed, apply the smallest fix under the sdlc:bug-fix discipline, prepare an expedited deployment with rollback, then merge the fix back into the default branch and every active release branch (patterns from the workspace settings) so it is not lost. Use when a production defect cannot wait for the next scheduled release.
argument-hint: "[ticket-id] [repo]"
---

# Hotfix

Fix production quickly without skipping the safeguards that matter. Follow every
rule in `[workspace]\CLAUDE.md`. The human creates branches; pushes and
merges each need explicit approval; the human runs builds, tests and deployments.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Source control" and "Environments and releases" sections of
   `[workspace]\workspace-settings.md` — default, release and hotfix
   branch patterns (including per-repo exceptions) and the promotion order.
3. Read the repo's `bootstrap.md` and `known-issues.md`.
4. If the problem is not yet understood, run the `sdlc:incident-triage` skill
   first.
5. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- The production defect and its impact.
- What is deployed in production: release branch/version or commit.
- Approval channel / change process for an expedited deployment.

Ask for any that are missing before Step 1.

## Step 1 — Confirm the deployed baseline
Identify the exact branch/commit in production (pipeline history, release notes, or
human confirmation). The hotfix branches from that, not from the default branch, unless they
are identical.

| Item | Value |
|---|---|
| Production version / commit | |
| Default branch contains unreleased changes? | |
| Active release branches | |
| Proposed branch | settings' hotfix pattern, from [base] |

Do not create the branch. Give the human the exact branch name and base, wait
for them to create and check it out, then verify with read-only git commands
(current branch, base commit matches the production baseline) before Step 2.

## Step 2 — Fix under bug-fix discipline
Run the `sdlc:bug-fix` skill Steps 1–7 on the hotfix branch. Keep the change to the
minimum that resolves the production impact; anything broader is recorded under
"Observations for later" for a normal ticket.

## Step 3 — Deployment and rollback plan

| Item | Detail |
|---|---|
| Deployment order (if more than one repo/DB) | |
| Pre-deploy steps | |
| Verification in production | |
| Rollback trigger | |
| Rollback method (previous artefact / revert) | |

## Step 4 — PR
Say the branch is ready for a PR to the deployed base branch. Give the title
(with "HOTFIX") and the Step 3 plan as deployment notes for its description.
If the settings' working agreement lets Claude raise PRs, offer `sdlc:raise-pr`
for it; otherwise the human raises it.

## Step 5 — Merge back
After production verification, list every branch that needs the fix: the default
branch and each active release branch. For each, state the merge or cherry-pick and any
conflicts expected. Each merge is a separately approved action, or done by the
human.

| Target branch | Method | Status |
|---|---|---|

## Completion summary
- Root cause and fix.
- Production deployment and verification result (as reported by the human).
- Branches the fix has been merged into, and any outstanding.
- Follow-up ticket for anything deferred.

Then run the `sdlc:context-update` skill (`known-issues.md`, `change-journal.md`,
`bootstrap.md`).
