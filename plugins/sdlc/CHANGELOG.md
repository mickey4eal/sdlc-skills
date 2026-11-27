# Changelog

Semantic versions. Patch: wording or trigger fixes. Minor: a new step or skill.
Major: a renamed skill or a changed workspace layout.

## 0.2.0 — 2026-11-27
- New skills: `config-change` (config-only changes, environments from the
  settings) and `env-verification` (manual verification of a delivered ticket).
- Applied the 2026-11-26 skill retro to `bug-fix`, `code-review`,
  `context-update`, `hotfix`, `raise-pr`, `refactor`, `skill-retro`, `spike`,
  `test-authoring` and `ticket-analysis` (test-as-repro, trace every branch,
  behaviour tables, ticket re-fetch, rebase survival check, `note` severity,
  batched context approvals, constraint confirmation, pending tests count as
  failures, built-in browser, data-store switch check).
- New "Working agreement" section in `workspace-settings.md`: branches,
  commits, pushes and PRs are the human's by default; `raise-pr`, `hotfix` and
  `skill-retro` follow it. CLAUDE.md template states the rule.
- `impact-analysis` gains a Writers check.
- CLAUDE.md template: load the skill before investigating; session prompts for
  the new skills.

## 0.1.0 — 2026-11-21
- First plugin release, ported from the 24 `-m` workspace skills.
- Skills renamed without the `-m` suffix; cross-references use `sdlc:<skill>`.
- Hard-coded `C:\Dev\ai-workspace\` paths replaced by `[workspace]`, resolved
  from `.claude/sdlc.md` (project, parent folders, then `~/.claude/sdlc.md`).
- New `impact-analysis` skill (generic; consumer paths come from
  `workspace-settings.md`).
- New `setup-workspace` skill with the workspace templates.
- References to workplace-only skills (`db-migration`, `contract-change`)
  replaced with generic guidance or optional project-skill pointers.
