---
name: setup-workspace
description: Scaffold or repair the SDLC workspace the sdlc skills work in — create the workspace folders, copy the plugin's templates (CLAUDE.md, workspace-settings.md, session-prompts.md, skill-feedback.md, task template, repo bootstrap), write the .claude/sdlc.md pointer file, and fill in the workspace settings with the human. Use when a sdlc skill reports that .claude/sdlc.md is missing, when setting up the plugin on a new machine or project, when adding a repo's context folder, or when the human asks to set up or repair the workspace.
argument-hint: "[workspace root] [pointer scope: user | project]"
---

# Set Up the Workspace

Create the workspace the other `sdlc:` skills read and write. Every file is
proposed before it is written, and nothing existing is overwritten without
explicit approval.

Templates live next to this file in `templates\` (this skill's directory).

## Before starting
1. Look for an existing pointer: `.claude/sdlc.md` in the current project or a
   parent folder, then `~/.claude/sdlc.md`. If one exists, read it and run in
   **repair** mode: only create what is missing and offer to update outdated
   files; never replace a filled-in file with a template.
2. If `$ARGUMENTS` names a workspace root, use it; otherwise propose one
   (default `C:\Dev\ai-workspace` on Windows, `~/dev/ai-workspace` elsewhere)
   and wait for approval.

## Inputs required from the human
- Workspace root (absolute path).
- Repos root: the folder(s) where the repositories live.
- Pointer scope: `user` (`~/.claude/sdlc.md`, covers every project — the
  usual choice for one person) or `project` (`<project>/.claude/sdlc.md`,
  commit it so a team shares it).
- Plugin repo: the local clone of the plugin's source repo, if any (used by
  `sdlc:skill-retro`; leave blank if the plugin is only installed).

## Step 1 — Plan
List what will be created and what already exists.

| Path | Action (create / exists — keep / exists — offer update) | Source template |
|---|---|---|

Folders: `ai-context\global\`, `ai-context\repos\`, `tasks\`, `scratch\`,
`playbooks\`.

Files:
| Workspace file | Template |
|---|---|
| `CLAUDE.md` | `templates\CLAUDE.md` |
| `workspace-settings.md` | `templates\workspace-settings.md` |
| `session-prompts.md` | `templates\session-prompts.md` |
| `skill-feedback.md` | `templates\skill-feedback.md` |
| `tasks\task-template.md` | `templates\task-template.md` |
| `ai-context\repos\bootstrap.template.md` | `templates\repo-context\bootstrap.md` |

Wait for approval.

## Step 2 — Create folders and copy templates
Create the approved folders and copy the approved files verbatim (the
`[workspace]` placeholders inside them stay — skills resolve them through the
pointer). Report each path written.

## Step 3 — Write the pointer file
Propose, then write, the pointer at the approved scope:

```markdown
# SDLC workspace pointer
Read by the sdlc plugin skills to resolve [workspace].

- Workspace root: `<absolute workspace root>`
- Repos root: `<absolute path(s)>`
- Plugin repo: `<absolute path or "none">`
```

## Step 4 — Fill in workspace-settings.md
Walk the human through `workspace-settings.md` one section at a time (working
agreement first — offer each line's bracketed default or the alternative —
then issue tracker, source control, environments and releases, hosting and observability,
security stack, consumer paths, performance tooling). Offer values found on the
machine (e.g. `gh auth status`, git remotes, project files) as suggestions, ask
for the rest, and leave an explicit `[unknown]` rather than guessing. Show each
section's proposed text; write after approval.

## Step 5 — Repo context (optional)
For each repo the human names, create
`ai-context\repos\[repo-name]\bootstrap.md` from the bootstrap template, then
offer to run `sdlc:context-first` on it.

## Completion summary
- Workspace root, repos root and pointer path.
- Files created, kept and updated.
- Settings sections still `[unknown]`.
- Suggested first prompt from `session-prompts.md`.
