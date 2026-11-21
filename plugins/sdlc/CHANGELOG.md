# Changelog

Semantic versions. Patch: wording or trigger fixes. Minor: a new step or skill.
Major: a renamed skill or a changed workspace layout.

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
