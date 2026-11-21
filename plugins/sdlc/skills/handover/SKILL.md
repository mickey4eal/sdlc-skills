---
name: handover
description: Write a session or ticket handoff document in [workspace]\tasks\[ticket-id]\ so another engineer or a fresh Claude session can resume exactly where work stopped — working agreement, immediate next step, current state, key facts and decisions, done so far, open items and reference docs. Use at the end of a long session, before context runs out, when pausing a multi-phase ticket, or when handing work to a colleague.
argument-hint: "[ticket-id]"
---

# Handover

Make the next session productive from its first message. Writes only to the
ticket's task folder. Follow every rule in `[workspace]\CLAUDE.md`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read everything in `[workspace]\tasks\[ticket-id]\`, including any
   previous handoff — this one replaces it as "current".
3. Read the repo's `bootstrap.md`.
4. Use the most recent `*-Handoff.md` under `[workspace]\tasks\` as the
   format reference if one exists; otherwise use the sections in Step 2.

## Inputs required from the human
- Ticket id.
- Audience: a fresh Claude session, a named colleague, or both.

Ask for any that are missing before Step 1.

## Step 1 — Gather state
From this session and the repos (read-only git inspection), collect:

| Item | Value |
|---|---|
| Branch(es) and last commit per repo | |
| Uncommitted changes | |
| Last build / test result (as reported by the human) | |
| Current step of the active skill | |
| Pending approvals | |

## Step 2 — Draft
Sections:
- **Working agreement** — mode, approval vocabulary, who runs builds and tests,
  any session-specific rules agreed.
- **Immediate next step** — one concrete action, with file and skill step.
- **Current state** — what exists now, per repo.
- **Key facts / decisions** — including rules ratified this session and why.
- **Done so far** — completed steps, briefly.
- **Open items** — questions, blockers, observations for later.
- **Reference docs** — task files, context files, ADRs, PRs.
- **Resume prompt** — a ready-to-paste opening prompt in the
  `session-prompts.md` format (Repo / Mode / Task / Skill / Ticket).

Keep it short enough to read in two minutes; link rather than repeat.
Present the draft. Wait for approval, then write to
`tasks\[ticket-id]\[ticket-id]-Handoff.md` (or the name the human gives).

## Completion summary
- Handoff path.
- The immediate next step.
- Anything that should also go into durable context via `sdlc:context-update`.
