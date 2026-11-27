---
name: skill-retro
description: Periodic retrospective that turns lessons learnt into reviewed changes to the sdlc plugin skills (in the plugin repo named in .claude/sdlc.md) and any project skills the workspace adds — read the skill-feedback inbox, memories and recent ai-context changes, group lessons by skill, find conflicts between skills, CLAUDE.md and memory, propose edits for approval, apply them, close the inbox entries and list the changed files with a suggested commit message (committing only if the working agreement allows). Use for the weekly skill retro, when the human asks to update skills from feedback, or when several feedback entries are open for one skill.
argument-hint: "[since date]"
---

# Skill Retro

Improve the workspace skills from real use, without bloating them or letting
them drift from `CLAUDE.md` and memory. This is the only skill that edits other
skills. Follow every rule in `[workspace]\CLAUDE.md` (approval
vocabulary, one change at a time). Best run in `Mode: analysis`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\skill-feedback.md` (open entries).
3. Read the memory index and memory files, and `CLAUDE.md`'s Skill library.
4. Note the date of the last retro (latest `applied` / `rejected` date in the
   inbox; `.claude\skills\` is not tracked in git) — the review covers
   everything since.

## Inputs required from the human
- Review window, if not "since the last retro".
- Any lessons not yet in the inbox.

## Step 1 — Gather
Collect candidate lessons from:
- Open inbox entries.
- Memories written or changed since the last retro that describe how a workflow
  should run (these are often skill changes recorded in the wrong place).
- `ai-context` changes since the last retro (`git log` plus `git status` /
  `git diff` on `[workspace]\ai-context` — most context changes are
  left uncommitted) that contradict a skill's paths, commands or steps.
- If the session-transcript search tool is available and the human agrees:
  recent sessions where the human corrected how a skill ran.

| # | Source | Skill / step | Lesson |
|---|---|---|---|

## Step 2 — Triage
Group by skill. For each lesson decide:
- `change` — a repeat occurrence, or a clear rule the human stated.
- `one-off` — happened once, situation-specific → reject with reason.
- `misplaced` — repo fact or personal preference → name the right destination
  (`ai-context` file, memory, `CLAUDE.md`) and reject here.
- `defer` — needs more evidence; state what would decide it.

| # | Skill | Decision | Reason |
|---|---|---|---|

Wait for approval of the triage.

## Step 3 — Conflict check
Compare every skill touched, plus `CLAUDE.md` and memory, for contradictions:
who creates branches, who runs builds, file locations, naming, approval gates,
cross-references to renamed skills. List each conflict and which source should
win.

| Conflict | Source A | Source B | Resolution |
|---|---|---|---|

## Step 4 — Propose edits, one skill at a time
For each skill with approved changes, describe the edit: section, current
wording (short quote), new wording, and the lessons it closes. Rules:
- Replace or sharpen an existing step rather than appending caveats.
- Keep skills generic — no ticket ids, repo-specific facts beyond the repo the
  skill is about, or one-off details.
- If the lesson is a `trigger` lesson, change the `description:` first — it
  decides when the skill is chosen.
- Keep cross-references and `CLAUDE.md` / `session-prompts.md` in step with any
  rename.

Wait for approval per skill, then apply.

## Step 5 — Check
For each changed skill, re-read it end to end for broken step numbering,
dangling references and contradictions with other skills. For heavily used
skills (`sdlc:bug-fix`, `sdlc:code-review`, `sdlc:ticket-analysis`), offer to run a few
test prompts with the `anthropic-skills:skill-creator` skill to confirm the
skill still triggers and behaves as intended.

## Step 6 — Close out
- Move each processed inbox entry to "Closed" with status `applied [date]`,
  `rejected` (reason) or `deferred` (what would decide it).
- Plugin skills are edited in the plugin repo (`Plugin repo` in
  `.claude/sdlc.md`), never in the installed plugin cache. Propose there: the
  skill edits, a `CHANGELOG.md` entry and a `version` bump in
  `plugins/sdlc/.claude-plugin/plugin.json` (patch: wording or trigger fix;
  minor: new step or skill; major: renamed skill or changed workspace layout).
  Commit message `Skill retro [date]: [skills changed]`, one line per lesson.
- Project skills the workspace adds (outside the plugin) are committed in their
  own repo; `skill-feedback.md` stays in the workspace.
- List the changed files and the suggested message. If the settings' working
  agreement says the human commits (the default), do not commit or offer to.
  If it lets Claude commit on approval, ask, then commit; nothing is pushed
  without separate approval. After a push, remind the human to update the
  plugin (`/plugin` → marketplace update).

## Completion summary
- Lessons reviewed, by decision.
- Skills changed and what changed.
- Conflicts resolved.
- Entries deferred and why.
- Changed files and suggested commit message (and commit status, if Claude
  committed).
