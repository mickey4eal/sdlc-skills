---
name: engineering-standards
description: Long-form engineering rules for work in workspace repositories — general principles, context-doc rules, code standards, repository-understanding checklist, task execution flow, change discipline, verification and documentation expectations. Use when planning or making any application code change in a workspace repo, or when deciding which ai-context files to update. [workspace]\CLAUDE.md is authoritative where the two conflict.
user-invocable: false
---

# Engineering Agent Rules

`[workspace]\CLAUDE.md` is authoritative where it conflicts with this
skill — in particular, the human runs all builds and tests and every change
waits for explicit approval.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## General Principles
- Prefer minimal, focused diffs.
- Preserve existing architecture unless the task explicitly calls for changing it.
- Match the repository's existing code style, naming, and module boundaries.
- Never rewrite working systems unnecessarily.
- Favor boring, maintainable solutions over clever ones.
- Run relevant tests, lint, or build checks before concluding work when feasible.
- If verification cannot be run, record why and name the residual risk.

## Context Rules
- Read existing AI context docs before starting application code work.
- Start with `ai-context/repos/<repo>/architecture.md`, `build-system.md`, `conventions.md`, and `testing-strategy.md`.
- Use the `sdlc:context-first` skill before code changes.
- Update context docs when architecture, commands, conventions, dependencies, or domain understanding changes.
- Append persistent operational discoveries to `ai-context/repos/<repo>/session-notes.md`.
- Append dated historical intent for meaningful changes to `ai-context/repos/<repo>/change-journal.md`.
- Record assumptions explicitly, especially when a command, dependency, or ownership boundary has not been verified.
- Keep temporary investigation notes in `scratch/`; promote only durable findings into `ai-context/`.

All `ai-context/` and `scratch/` paths are under `[workspace]\`.

## Code Standards
- Prefer typed interfaces and explicit contracts where the repository already uses them.
- Avoid premature abstraction.
- Add comments only when logic is non-obvious or a future maintainer needs context that code alone cannot show.
- Reuse existing helpers, dependency injection patterns, test fixtures, and error-handling conventions.
- Keep behavior changes close to the impacted module.
- Do not mix unrelated refactors with feature or bug-fix work.
- Protect user data, credentials, and environment-specific settings.

## Repository Understanding Checklist
Before modifying application code, identify:

1. Repository structure and important modules.
2. Frameworks, libraries, runtime targets, and deployment shape.
3. Build, test, lint, and run commands.
4. Architectural patterns and ownership boundaries.
5. Coding conventions and naming patterns.
6. Entry points, API surfaces, functions, queues, jobs, or scheduled tasks.
7. Relevant tests and fixtures.
8. Context documents that should be updated after the work.

## Task Execution Flow
1. Read the task and clarify the intended outcome.
2. Read the relevant repo context docs.
3. Inspect impacted files and nearby tests.
4. Propose a short plan when the work is non-trivial.
5. Implement incrementally with minimal diffs.
6. Run relevant tests, lint, or build checks.
7. Update context docs with durable findings.
8. Add a change-journal entry when the change has future architectural or operational relevance.
9. Summarize changes, verification, and any remaining risk.

## Change Discipline
- Do not modify application code during context-building unless explicitly requested.
- Do not revert unrelated user changes.
- Do not introduce new frameworks, packages, or broad abstractions without a clear need.
- Prefer adding or updating targeted tests when behavior changes.
- Keep generated files, scratch output, and local environment changes out of application commits unless required.

## Verification Expectations
- Prefer the smallest command that proves the changed behavior.
- For .NET repositories, look first for solution or project-level `dotnet test` commands in `build-system.md`.
- For Node repositories, use existing `package.json` scripts.
- For integration tests, confirm required local settings and external dependencies before running.
- Capture failed verification honestly, including command attempted and likely cause.

## Documentation Expectations
- Update `ai-context/repos/<repo>/build-system.md` after verifying commands.
- Update `architecture.md` when module boundaries, entry points, deployment shape, or integration patterns become clearer.
- Update `dependency-map.md` when adding or discovering dependencies, queues, topics, databases, APIs, or storage.
- Update `conventions.md` when a repeated local pattern is confirmed.
- Update `known-issues.md` for recurring failures, flaky tests, migration hazards, or unresolved risks.
- Update `session-notes.md` for architectural discoveries, recurring pain points, undocumented conventions, unfinished refactors, and fragile areas that should persist across sessions.
- Update `change-journal.md` for dated change intent: what changed, why it changed, verification, and follow-ups.
