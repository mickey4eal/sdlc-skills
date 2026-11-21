---
name: security-review
description: Read-only security review of a change or area in a workspace repository against the workspace's real risks — token authentication and permission checks, managed identity and secret stores, secrets in config/pipelines, SQL injection in data access and stored procedures, input validation, PII in logs, CORS, and dependency vulnerabilities — producing a severity-ranked findings table. Use before merging security-sensitive changes or when auditing an area. Prefer this over the built-in security-review for workspace repos.
argument-hint: "[branch | PR | path]"
---

# Security Review

Find real security weaknesses in a change or area. Read-only — no code changes;
accepted findings are fixed under the relevant skill. Follow every rule in
`[workspace]\CLAUDE.md`. Best run in `Mode: analysis`.

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the "Security stack" section of
   `[workspace]\workspace-settings.md` — it names the concrete
   mechanisms each dimension below is checked against.
3. Read the repo's `bootstrap.md`, `architecture.md` and `dependency-map.md`.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- The target: branch, PR, diff, or path set.
- Whether the area is internet-facing, internal only, or behind the API gateway
  named in the settings.

Ask for any that are missing before Step 1.

## Step 1 — Scope and trust boundaries
Identify entry points in scope and where untrusted input enters.

| Entry point | Exposure | Auth | Input sources |
|---|---|---|---|

## Step 2 — Review against dimensions
For each, state "no issue" or a specific finding with file:line. Do not pad.

- **Authentication** — token validation configuration (authority, audience,
  issuer), anonymous endpoints, function auth levels.
- **Authorisation** — every write/read checks the right permission using the
  settings' authorisation mechanism; no IDOR via ids in routes; role checks
  enforced server-side, not just in the UI.
- **Secrets** — none in source, app settings, pipeline YAML, tests, or logs;
  the settings' secret store and identity used; credential files untracked.
- **Injection** — parameterised data access only, no string-built SQL; dynamic
  SQL in stored procedures is parameterised; no command/path injection.
- **Input validation** — validators on request contracts; size limits; enum and
  id range checks.
- **Data exposure** — PII or sensitive fields in responses, logs, exceptions,
  URLs or telemetry; error responses do not leak internals.
- **Transport and browser** — HTTPS only, CORS origins restricted, frontend
  tokens not stored insecurely.
- **Messaging** — message handlers validate payloads; poison-message handling;
  no trust in message-supplied identity without verification.
- **Dependencies** — known-vulnerable packages (ask the human to run the
  settings' vulnerability checks and paste the output).

## Step 3 — Findings

| Severity | File:line | Finding | Exploit scenario | Recommendation |
|---|---|---|---|---|

Severity values: `critical`, `high`, `medium`, `low`. Every finding needs a
concrete exploit scenario; if there are none, say so plainly.

## Step 4 — Disposition
The human decides which findings to act on, using the approval vocabulary.
Accepted fixes proceed under `sdlc:bug-fix` (or `sdlc:hotfix` if exposed in PROD).

## Completion
Summarise findings by severity, accepted vs deferred. Deferred findings are
recorded under "Observations for later" for `known-issues.md`.
