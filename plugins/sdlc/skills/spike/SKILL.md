---
name: spike
description: Time-boxed investigation in the workspace to answer a specific question or de-risk a decision — frame the question and success criteria, investigate (prototypes only in [workspace]\scratch), present options with honest trade-offs, recommend, and record the outcome. No production code changes. Use for feasibility questions, intermittent or poorly understood problems, comparing design or dependency options, or surfacing unknowns before larger work.
argument-hint: "[question] [time box]"
---

# Spike / Investigation

Time-boxed investigation to answer a specific question or de-risk a decision. The
deliverable is understanding — findings, options, and a recommendation — **not**
production code. Follow every rule in `[workspace]\CLAUDE.md`. Best run
in `Mode: analysis`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md` for
   each repo the question touches.
3. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- The question the spike must answer.
- The time box (how much effort this warrants).
- The decision or work this feeds into.
- What "done" looks like — the criteria that would let the decision be made.

Ask for any that are missing before Step 1.

## Hard rule for this skill
No production code changes. Any prototype exists only to answer the question and
lives under `[workspace]\scratch\` — it is throwaway and never merged.
If the investigation concludes that code should change, that is a *separate*
session under the appropriate skill.

## Step 1 — Frame the question and success criteria
Restate the question precisely and the criteria that will close it. List the
constraints that will rank the options: which risks or exposures must be closed
and which may stay open. Confirm each one with the human rather than assuming
it. Confirm the time box. Wait for approval before investigating — a mis-framed spike wastes the
box.

## Step 2 — Investigate
Read code, docs, and history relevant to the question. Prototype only if
necessary, and only in `scratch\`. Keep a running record of what was examined so
the conclusion is auditable.

| Examined | What it told us |
|---|---|

Stop when the success criteria are met or the time box is spent — whichever
comes first. If the box is spent without an answer, that itself is a finding.

## Step 3 — Findings and options
Present what was learned. Where the spike compares approaches, lay them out with
honest trade-offs — do not pre-load the recommendation into the table.

| Option | How it works | Pros | Cons | Effort / risk |
|---|---|---|---|---|

## Step 4 — Recommendation
State the recommended path and why, referencing the Step 1 criteria. Note the
key risks and what remains unknown. If the answer is "not feasible" or "need
more information", say so plainly.

## Step 5 — Record the outcome
Write the findings and recommendation where the downstream decision can use them:
a `[workspace]\tasks\[ticket-id]\` note if one exists, otherwise a short
artefact under `scratch\` or the relevant `ai-context` file. Delete or clearly
mark any throwaway prototype. Explicitly confirm: no production code was changed.

## Completion
- The question and whether it was answered.
- Recommendation and key risks.
- Where the outcome was recorded.
- Follow-up work proposed (as a separate session), if any.

No `sdlc:context-update` run is required unless the spike produced a durable context
artefact worth indexing.
