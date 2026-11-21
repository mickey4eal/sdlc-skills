---
name: context-first
description: Pre-code understanding pass for a workspace repository — analyse structure, frameworks, build/test commands, architecture, conventions and entry points, and record confirmed knowledge in [workspace]\ai-context\repos\[repo]\. Use before modifying application code in a repo whose context is missing or stale, when onboarding to an unfamiliar repo, or when another workspace skill says to run the sdlc:context-first pass.
argument-hint: "[repo-name]"
---

# Context-First Workflow

Follow every rule in `[workspace]\CLAUDE.md`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Rule
Before modifying application code, understand and document the relevant repository.

## Before starting
1. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md` and any
   existing context files in that folder.
2. Treat those files as the starting point; this pass confirms, corrects and
   extends them.

## Required Pass
1. Analyze repository structure.
2. Identify frameworks, libraries, and runtime shape.
3. Identify build, test, and lint commands from project files, package scripts, README, and pipeline files.
4. Identify architectural patterns and module boundaries.
5. Identify coding conventions from local examples.
6. Identify important modules and entry points.
7. Update `[workspace]\ai-context\repos\[repo-name]\` with confirmed durable knowledge.

## Boundaries
- Do not modify application code during context-building unless explicitly requested.
- Keep context edits in `[workspace]\ai-context` and supporting workflow notes in `[workspace]\playbooks`.
- Mark unverified commands as candidates until they have been run.
