---
name: adr
description: Record an architecture or design decision for the workspace as an ADR — context, options considered with honest trade-offs, the decision, consequences and status — stored with the repo's ai-context (or global context for cross-repo decisions). Use when a design choice is made or ratified (e.g. a business rule, pattern, technology or structure), when a sdlc:spike concludes, or when asked to document why something is the way it is.
argument-hint: "[decision title]"
---

# Architecture Decision Record

Capture a decision so future sessions and engineers know what was decided and
why. Writes only to `[workspace]\ai-context\`. Follow every rule in
`[workspace]\CLAUDE.md`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the affected repo's `bootstrap.md` (open decisions) and
   `architecture.md`.
3. Check for existing ADRs in `ai-context\repos\[repo]\decisions\` and
   `ai-context\global\decisions\`; if this supersedes one, read it.
4. Read any spike outcome or task notes that fed the decision.

## Inputs required from the human
- The decision (or the question if not yet decided).
- Who ratified it and when, if already decided.
- Scope: single repo or cross-repo.

Ask for any that are missing before Step 1.

## Step 1 — Location and number
Repo-scoped: `ai-context\repos\[repo]\decisions\NNNN-[kebab-title].md`.
Cross-repo: `ai-context\global\decisions\NNNN-[kebab-title].md`.
`NNNN` is the next number in that folder. If the folder does not exist, propose
creating it. Wait for approval.

## Step 2 — Draft
Sections:
- **Title** and **Status** — `proposed`, `accepted`, `superseded by NNNN`,
  `deprecated`.
- **Date** and **Ticket**.
- **Context** — the problem and forces, with file paths or evidence.
- **Options considered** — table, trade-offs stated honestly.

  | Option | Pros | Cons |
  |---|---|---|

- **Decision** — the rule or choice, stated precisely enough to apply.
- **Consequences** — what becomes easier, harder, or must now be true;
  follow-up work.
- **References** — tickets, task files, code.

Present the draft. Wait for approval, then write.

## Step 3 — Link it
Propose updates (each approved separately): the repo's `bootstrap.md` (remove
from open decisions, link the ADR), `architecture.md` or `conventions.md` if the
decision changes them, and the superseded ADR's status.

## Completion summary
- ADR path and status.
- Files updated to reference it.
