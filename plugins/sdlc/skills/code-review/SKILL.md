---
name: code-review
description: Read-only review of a branch, PR, diff or file set in a workspace repository before it merges — scope and baseline, review against correctness, contract preservation, error handling, security, data access/performance, tests and consistency, then a severity-ranked findings table. Use when a change needs a review pass before merge in this workspace. Prefer this over the built-in code-review and engineering:code-review skills for workspace repos.
argument-hint: "[branch | PR | path]"
---

# Review Code Changes

Review a change before it merges. This is a read-only analysis pass that
produces findings — it does not modify code. Any finding the human elects to fix
is then carried out under the relevant workspace skill or the universal hard
rules in `[workspace]\CLAUDE.md`. Best run in `Mode: analysis`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md` and
   `conventions.md` for the repo under review.
3. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists. If it holds a `ticket-snapshot.md`, re-fetch the ticket as in
   `sdlc:ticket-analysis` Step 0 and report what changed since the snapshot.
   Review against the current ticket, and flag changed acceptance criteria
   before Step 1. If the re-fetch hits a sign-in page, ask the human to sign
   in; continue against the snapshot only if they say so, and state that in
   the Completion summary.

## Inputs required from the human
- The review target: branch name, PR, diff, or file/path set.
- The intended behaviour of the change (what it is supposed to do).
- The contract that must be preserved (API, schema, status codes, auth), if any.

Ask for any that are missing before Step 1.

## Step 1 — Establish scope and baseline
State exactly what changed and what the change is meant to achieve. Read the
diff and the surrounding code it touches. Do not review files outside the diff
unless the change's correctness depends on them. If the branch was moved to a
newer base, list the base's commits that touch the changed files
(`git log <old-base>..<new-base> -- <files>`) and check each one's changes
survived and still hold; add a row for any that did not.

| File | Nature of change | Intended effect |
|---|---|---|

## Step 2 — Review against dimensions
Assess the change on each dimension below. For each, state either "no issue" or
a specific finding with a file/line reference. Do not pad with generic advice.

- **Correctness** — does it do what it claims? Off-by-one, null/empty, ordering,
  concurrency, boundary conditions.
- **Contract preservation** — routes, request/response shapes, status codes,
  auth, serialization, public type signatures unchanged unless intended.
- **Error handling** — failures caught at the right level; exception types
  callers expect; no swallowed errors; resource disposal.
- **Security** — input validation, injection surfaces, authorization checks,
  secrets handling, sensitive data in logs/URLs.
- **Data access / performance** — N+1 queries, unbounded results, missing async,
  avoidable allocations on hot paths.
- **Tests** — new/changed behaviour is covered; edge and failure cases; tests
  assert behaviour, not implementation detail.
- **Consistency** — follows existing patterns and conventions in this codebase;
  no new pattern where an established one exists.

## Step 3 — Findings
One row per finding, most severe first. Be specific and actionable.

| Severity | File:line | Finding | Recommendation |
|---|---|---|---|

Severity values: `blocker` (must fix before merge), `major` (should fix),
`minor` (optional), `nit` (style/preference), `note` (matches the earlier
version's behaviour; not ranked). If there are no findings, say so plainly — do
not invent issues.

In a repo that rewrites an earlier version (e.g. a V2 rewrite of a V1 service),
check the earlier version's behaviour in git history before ranking a finding.
A finding that matches it is a `note`, not a severity.

## Step 4 — Disposition
The human decides which findings to act on, using the approval vocabulary. For
each finding accepted as a fix, proceed under the relevant skill (`sdlc:bug-fix`,
`sdlc:refactor`, etc.) or the universal hard rules — propose the change, wait for
approval, apply, build gate.

## Completion
Summarise: files reviewed, findings by severity (and any reported as `note`),
which were accepted for fix vs deferred, and whether the review ran against
the snapshot because the ticket could not be re-fetched. If any code was changed as a result, run the `sdlc:context-update`
skill for the affected artefacts; a pure review with no code change needs no
context update.
