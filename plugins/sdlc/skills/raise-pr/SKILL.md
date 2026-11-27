---
name: raise-pr
description: Prepare a pull request for a workspace repository on the git host named in the workspace settings (e.g. GitHub) — verify the branch and target (default or release branch), check the diff is in scope and clean, then draft the title and description from the diff, ticket and task folder (summary, changes, testing, deployment notes, risks). By default only drafts — the human pushes and raises the PR — unless the workspace settings' working agreement lets Claude raise PRs on approval. Use when the human asks for a PR description or to raise a PR.
argument-hint: "[ticket-id] [target branch]"
---

# Raise a Pull Request

Get a change ready for review and describe it well. Follow every rule in
`[workspace]\CLAUDE.md`. Who pushes and creates the PR comes from the
"Working agreement" section of `[workspace]\workspace-settings.md`. If it is
missing or says the human does, this skill only drafts.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Source control" and "Issue tracker" sections of
   `[workspace]\workspace-settings.md` — host, CLI, branch patterns, PR
   title format, ticket URL.
3. Read the repo's `bootstrap.md`.
4. Read `[workspace]\tasks\[ticket-id]\` if it exists.

## Inputs required from the human
- Repo and ticket id.
- Target branch: the default branch or the active release branch (patterns in
  the settings, including any per-repo exception).
- Reviewers, if any.

Ask for any that are missing before Step 1.

## Step 1 — Branch check
Read-only git inspection. Report:

| Check | Result |
|---|---|
| Current branch | |
| Branch name follows the settings' feature (or hotfix) pattern | |
| Target branch exists on origin | |
| Commits ahead / behind target | |
| Uncommitted changes | |
| Branch pushed to origin | |

If behind the target, recommend how to bring it up to date; do not merge or
rebase without approval.

## Step 2 — Scope check
List every changed file against the target. Flag anything outside the ticket's
scope, generated files, local settings, secrets, `*.env`, debug code, or
commented-out code.

| File | In scope? | Note |
|---|---|---|

Wait for the human to resolve flags before drafting.

## Step 3 — Pre-PR checklist

| Item | Status |
|---|---|
| Build clean (as reported by the human) | |
| Tests pass (as reported by the human) | |
| New/changed behaviour covered by tests | |
| `sdlc:code-review` run, blockers resolved | |
| Contract changes identified (`sdlc:impact-analysis`) | |
| DB / config / infra steps needed for deploy | |
| `sdlc:context-update` run | |

## Step 4 — Draft title and description
Title: the settings' PR title format.

Description sections:
- **Summary** — what and why, in two or three sentences.
- **Changes** — grouped by area, not file-by-file.
- **Testing** — what was run and the result, as reported by the human; manual
  checks and environment.
- **Deployment notes** — order, pre/post-deploy steps, config or DB changes;
  "None" if none.
- **Risks / follow-ups** — known limitations, deferred items.
- Ticket link (the settings' ticket URL pattern).

End the description with the attribution line required by the session.
Present the draft and wait for approval or amendments.

## Step 5 — Write the description (and create, only if agreed)
On approval, write the approved title and description to
`[workspace]\tasks\[ticket-id]\pr-description.md`.

- Working agreement says **the human raises PRs** (the default): stop here. Do
  not push the branch or create the PR.
- Working agreement says **Claude raises PRs on approval**: ask separately to
  push the branch, then separately to create the PR with the settings' CLI
  (e.g. `gh pr create`) against the approved target with the approved title
  and body. Report the PR URL.

## Completion summary
- Description file path (and PR URL if Claude created it).
- Target branch.
- Checklist items still open.
