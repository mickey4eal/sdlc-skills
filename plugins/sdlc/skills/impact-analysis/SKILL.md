---
name: impact-analysis
description: Read-only analysis of who is affected when a contract changes in a workspace repository — HTTP API routes and schemas, client libraries, message/event payloads, database objects, search indexes, configuration keys or shared packages. Finds every consumer (from the workspace settings' consumer paths, the repos' dependency maps and a code search), classifies each change as breaking or compatible, and sets a safe deployment order. Use when a ticket, fix or refactor crosses a contract, when sdlc:ticket-analysis or sdlc:raise-pr asks for an impact analysis, or before removing or renaming anything another repo might use.
argument-hint: "[contract or change] [repo]"
---

# Impact Analysis

Find everything that depends on a contract before it changes, and say what
breaks, what does not, and in what order to ship. Read-only: no code changes.
Follow every rule in `[workspace]\CLAUDE.md`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read `[workspace]\workspace-settings.md` — the **Consumer paths** section
   lists where consumers of each contract type live (other repos, client
   packages, mobile apps, reports, external partners).
3. Read the producing repo's `[workspace]\ai-context\repos\[repo]\bootstrap.md`,
   `api-surface.md` and `dependency-map.md`, and
   `[workspace]\ai-context\global\dependency-architecture-graph.md` if present.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\`.

If the consumer paths are missing from the settings, say so, continue with the
dependency maps and code search, and propose the entries at the end.

## Inputs required from the human
- The contract and the intended change (or the diff / branch that makes it).
- The producing repo.

## Step 1 — Define the contract surface
State exactly what changes, at the level a consumer sees it.

| Contract type | Identifier | Current shape | Proposed shape |
|---|---|---|---|

Contract types: `http-api` (route, method, request/response schema, status
codes, auth), `client-lib` (public types and members), `message` (topic/queue,
payload fields), `database` (table, column, view, procedure, index),
`search-index` (index, field, analyser), `config` (key, default), `package`
(shared library version or API).

## Step 2 — Find consumers
For each contract item, search in this order and record where each consumer was
found:
1. Consumer paths from the workspace settings.
2. `dependency-map.md` of the producer and of known consumer repos.
3. A code search across the workspace repos for the identifier (route string,
   type or member name, topic name, column name, config key). State the search
   terms and the repos searched.
4. Anything outside the workspace the human knows about (external clients,
   reports, scheduled jobs) — ask if the settings name none.

| # | Consumer (repo / system) | Where (file or evidence) | Uses which item | Found via |
|---|---|---|---|---|

Read each consumer's usage before classifying it — a name match is not a use.

**Writers.** When the change fills, backfills, defaults or re-derives a value
(a new column, a migrated field, a setting that switches store), also list
every writer of that value — APIs, workers, imports, scheduled jobs, manual
scripts, other repos — per environment. A writer that keeps writing the old
shape after the change is a breaking item.

| # | Writer (repo / system) | Where | Writes which item | Environments |
|---|---|---|---|---|

## Step 3 — Classify each change

| Item | Change | Classification | Affected consumers | Why |
|---|---|---|---|---|

Classification values:
- `compatible` — additive or widening; existing consumers keep working
  (new optional field, new endpoint, new nullable column).
- `breaking` — removal, rename, type change, narrowing, new required input,
  changed status code, semantics change.
- `behavioural` — same shape, different meaning or timing; treat as breaking
  unless every consumer is confirmed unaffected.
- `unknown` — consumer could not be inspected; state what would decide it.

## Step 4 — Safe rollout
Propose the order that keeps every environment working at each step, e.g.:
- Expand → migrate consumers → contract (add the new shape, move consumers,
  remove the old shape last).
- Database before the code that needs new objects; code that stops using an
  object before the object is dropped.
- Producers of new message fields before consumers that read them; consumers
  tolerant of a field before the producer stops sending it.
- Versioned routes or feature flags when consumers cannot move together.

| Step | Repo | Change | Deploy after | Rollback |
|---|---|---|---|---|

Use the deployment order and environments from the workspace settings.

## Step 5 — Result
Write the result to `[workspace]\tasks\[ticket-id]\impact-analysis.md` when a
ticket folder exists (approval before writing); otherwise return it in chat.

- Contract surface (Step 1).
- Consumers found, with evidence, and the searches that found nothing.
- Breaking items and their consumers.
- Rollout order and rollback position.
- Open questions and `unknown` items.
- Proposed additions to the settings' **Consumer paths** or to
  `dependency-map.md` for consumers discovered here (applied later through
  `sdlc:context-update`).
