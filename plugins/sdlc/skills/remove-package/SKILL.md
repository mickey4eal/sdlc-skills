---
name: remove-package
description: Safely remove a dependency (NuGet, npm, or other) from a workspace repository and replace it with an alternative — audit every usage, flag behaviours the replacement must replicate, replace one usage per build gate, then remove references and verify zero matches. Use when a package is being retired, replaced, consolidated, or swapped for an in-house equivalent or a different library.
argument-hint: "[package-name] [replacement]"
---

# Remove a Package

Safely remove a dependency and replace it with an alternative. Follow every
rule in `[workspace]\CLAUDE.md` (approval vocabulary, one step at a
time, human runs builds and tests).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\ai-context\repos\[repo-name]\bootstrap.md`.
3. Run the `sdlc:context-first` skill before touching application code.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- Package name and the exact replacement (library, in-house type, or "inline").
- Scope: whole solution, or named projects only.

Ask for any that are missing before Step 1.

## Step 1 — Audit every usage
Enumerate, across the full solution, every point that touches the package.
Produce one table; do not edit anything yet.

| Usage | File | Project | Type of use |
|---|---|---|---|

Use type-of-use values: `call site`, `DI registration`, `using/import`,
`config reference`, `test usage`, `project/package reference`. Search by package
name, root namespace(s), and each primary public type. Record the search terms
used so coverage is auditable.

## Step 2 — Flag non-trivial behaviours
List every behaviour the replacement must replicate, one row each. These are the
things a naive swap would silently break.

| Behaviour | Where it occurs | What the replacement must do |
|---|---|---|

Examples to look for: retry/back-off policies, serialization settings, default
values, thread-safety/singleton assumptions, culture/timezone handling,
exception types callers catch, ordering guarantees, disposal semantics.
Wait for approval of this list before designing the replacement.

## Step 3 — Replacement plan
For each usage in Step 1, state the approach. Produce one table. Wait for
approval before implementing anything.

| Usage | Approach | Target file | DI change | Test change |
|---|---|---|---|---|

State explicitly any usage that cannot be replaced like-for-like and why.

## Step 4 — Implementation (one usage at a time)
For each approved replacement, in order:
1. Read the target file and the chosen reference pattern. State the reference.
2. Describe the proposed change. Wait for approval.
3. Apply the change.
4. Human runs the build. Interpret the output. Fix any failure (propose → approve
   → apply) before moving to the next usage.

Do not batch replacements. One usage, one build gate.

## Step 5 — Removal (each as a separate approved sub-step)
Only after all call sites are replaced and the build is clean. List the item,
wait for approval, remove, then continue:
1. DI registrations for the package's types.
2. Constructor injections / field declarations no longer used.
3. Class files / wrappers that existed only to host the package.
4. `using`/`import` statements left dangling.
5. Package reference in project files / manifest and any lock file entry.

## Step 6 — Verification
String-search the entire solution for the removed package name and each of its
primary public types. Report the search terms and the result. **Zero matches is
required.** If any match remains, return to Step 4 for that usage.

## Step 7 — Tests
Human runs the full test suite. Interpret the output. For each regression:
identify root cause, propose fix, wait for approval, apply, re-run.

## Step 8 — Completion summary
- Package removed and replacement adopted.
- Usages replaced (count) and behaviours preserved (from Step 2).
- Files removed (explicit list).
- Build and test status as reported by the human.
- Any deferred items recorded under "Observations for later".

Then run the `sdlc:context-update` skill to refresh `dependency-map.md`,
`change-journal.md`, and any affected context files.
