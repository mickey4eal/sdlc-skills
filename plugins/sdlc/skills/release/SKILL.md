---
name: release
description: Plan and document a release across workspace repositories — confirm every in-scope repo has its work merged to the release branch, collect changes from tickets and change journals, write release notes in the workspace's format (summary, pre-deployment steps, post-deployment steps, known issues, notes), set cross-repo deployment order and a per-environment checklist, and plan rollback or fix-forward. Use when cutting, preparing or documenting a release.
argument-hint: "[release version]"
---

# Prepare a Release

Make the release understandable and repeatable for whoever deploys it. Follow
every rule in `[workspace]\CLAUDE.md`. Claude does not merge, push, tag
or trigger pipelines without explicit approval for that specific action; the
human runs all deployments.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Source control" and "Environments and releases" sections of
   `[workspace]\workspace-settings.md` — branch patterns, promotion
   order, pipelines, release-notes location, format reference and sections.
3. Read the release-notes format reference the settings name.
4. Read the hosting reference named in the settings' "Hosting and
   observability" section.

## Inputs required from the human
- Release version (`x.y.z`) and the tickets in it.
- Repos in scope.
- Target environments and dates, if known.

Ask for any that are missing before Step 1.

## Step 1 — Release scope
Read-only git inspection per repo.

| Repo | Release branch exists | Tickets merged | Unmerged ticket branches | Ahead of default branch |
|---|---|---|---|---|

Use the settings' release-branch pattern, including any per-repo exception.

Flag any ticket in scope whose work is not on the release branch, and any change
on the release branch not tied to an in-scope ticket.

## Step 2 — Change collection
From each repo's `change-journal.md`, the task folders and the git log between
the default branch and the release branch:

| Ticket | Repo | Change | Config / DB / infra impact | Deployment step needed |
|---|---|---|---|---|

## Step 3 — Deployment order
Order repos by dependency, starting from the settings' usual deployment order.
Include infrastructure changes that run ahead of the app in the pipelines.

| Order | Repo | Pipeline | Precondition |
|---|---|---|---|

## Step 4 — Draft release notes
Use the settings' release-notes sections, matching the format reference. By
default:
- **Summary**
- **Pre-Deployment Steps** (per environment) — re-grants, config, data
  captures, coordination with other teams.
- **Post Deployment Steps** — verification per service, smoke tests, data
  checks.
- **Known Issues and How to Resolve**
- **Notes** — rollback position (rollback vs fix-forward, and why).

Present the draft. Wait for approval, then write to the settings'
release-notes location (or a folder the human names).

## Step 5 — Environment checklist

One row per environment, in the settings' promotion order.

| Environment | Pre-deploy done | Deployed (order) | Post-deploy verified | Issues |
|---|---|---|---|---|

The human fills this in as the release progresses; interpret any failure output
they provide (propose → approve → apply).

## Step 6 — Merge back
After the final environment, list the merges needed (release branch → default
branch per repo, any hotfixes into active release branches). Each merge is a separately approved
action, or is done by the human.

## Completion summary
- Release notes path.
- Repos and tickets released.
- Environment status.
- Outstanding merges and follow-ups.

Then run the `sdlc:context-update` skill for each repo whose bootstrap or
known-issues changed as a result of the release.
