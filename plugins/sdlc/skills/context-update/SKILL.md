---
name: context-update
description: Refresh all AI context artefacts in [workspace]\ai-context (global maps, per-repo architecture, api-surface, dependency-map, change-journal, known-issues, bootstrap) after a ticket's code work is complete and verified. Use at the end of every ticket, when the human says to update context or close out a ticket, or when another workspace skill's completion step says to run sdlc:context-update.
argument-hint: "[ticket-id] [repo-name]"
---

# Update Context After a Ticket

Refresh all AI context artefacts so the next session starts from accurate
state. Follow every rule in `[workspace]\CLAUDE.md`. All paths below
are relative to `[workspace]\`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Principles
- Update context when a service boundary, API contract, event, queue, database,
  deployment detail, build/test/lint command, convention, recurring issue, or
  meaningful change intent is discovered or changes.
- Prefer concise bullets with file paths or command evidence.
- Separate confirmed facts from hypotheses.
- Promote durable findings out of `scratch/`; delete temporary output.

## When to run
- After a ticket's code work is complete and verified, before closing it out.

## Files to review and update
Review each in order. For each: read current content, identify what this ticket
changed, show the proposed update, wait for approval, then write. Skip a file
only if genuinely unaffected — state that it was reviewed and unchanged.

Global:
- `ai-context\global\context-inventory.md`
- `ai-context\global\dependency-architecture-graph.md`
- `ai-context\global\repository-maps-index.md`

Per repo (`ai-context\repos\[repo]\`):
- `architecture.md`
- `api-surface.md`
- `dependency-map.md`
- `change-journal.md`
- `known-issues.md`
- `bootstrap.md`

Also update when affected (not in the mandatory walk above, but check):
`architecture-graph.md`, `build-system.md`, `conventions.md`,
`domain-models.md`, `repository-map.md`, `session-notes.md`,
`testing-strategy.md`, `glossary.md`.

## Procedure
1. Confirm the ticket scope and what actually changed (files, contracts,
   dependencies, behaviour). If a ticket id was given, read
   `tasks\[ticket-id]\` if it exists.
2. For each file above: read → identify delta → show proposed update → wait for
   approval → write. One file at a time.
3. Add a dated entry to `change-journal.md` for the completed ticket:
   - Ticket id and name.
   - What changed and why.
   - Verification performed (build/test status as reported by the human).
   - Follow-ups / deferred items.
4. Update the repository `bootstrap.md` to reflect the post-ticket state:
   current state, do-not-touch items, open decisions, open observations. This
   is always the last context file updated so it reflects everything else.
5. Lessons for skills — review the ticket for corrections the human made to
   how a workflow ran, steps that were skipped, reordered or added, a skill
   that was wrong, missing or not picked up when it should have been, and
   rules that conflicted. Classify each lesson:

   | Lesson | Kind | Destination |
   |---|---|---|

   Kind and destination:
   - `preference` (how the human works, in every repo) → memory, or
     `CLAUDE.md` if it is a hard rule.
   - `repo-fact` → this repo's context files (handled in step 2).
   - `workflow` (how a skill's steps should run) → skill-feedback inbox.
   - `trigger` (a skill was or was not chosen when it should have been) →
     skill-feedback inbox.

   Propose an entry in `[workspace]\skill-feedback.md` for each
   `workflow` and `trigger` lesson, with status `open`. Do not edit any skill
   here — skill changes are made only by the `sdlc:skill-retro` skill. If there
   are no lessons, say so.

## Completion summary
- Files updated (list) and files reviewed-unchanged (list).
- `change-journal.md` entry added.
- `bootstrap.md` refreshed.
- Skill-feedback entries added (count), or "no lessons".
