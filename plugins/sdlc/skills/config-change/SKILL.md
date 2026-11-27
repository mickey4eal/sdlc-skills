---
name: config-change
description: Configuration-only change in a workspace repository — infrastructure parameter files (e.g. Bicep/ARM, Terraform), app settings, environment files, secret-store references, pipeline variables, docker-compose values — with no application code change. Compare the value per environment, find what deploys it (including pipeline triggers that exclude the path), assess side effects when it takes effect, plan rollback by value and a post-deploy check. Use for config-only or infrastructure-parameter tickets. Not for code changes that also need config (the code skill's plan includes those).
argument-hint: "[ticket-id] [setting(s)]"
---

# Configuration Change

Change configuration safely when no application code changes. Follow every
rule in `[workspace]\CLAUDE.md` (approval vocabulary, one step at a
time, human runs all pipelines and deployments).

> **Workspace.** `[workspace]` below is the workspace root named in
> `.claude/sdlc.md`: look in the current project, then its parent folders,
> then `~/.claude/sdlc.md`. If none exists, stop and offer
> `/sdlc:setup-workspace`.

## Before starting
1. Read `[workspace]\CLAUDE.md` if not already read this session.
2. Read the repo's `bootstrap.md` and `architecture.md` (hosting, ops,
   pipelines).
3. Read the "Environments and releases" section of
   `[workspace]\workspace-settings.md`.
4. If a ticket id is known, read `[workspace]\tasks\[ticket-id]\` if it
   exists.

## Inputs required from the human
- The setting(s) to change and the reason (or the ticket).
- The target value per environment.

Ask for any that are missing before Step 1.

## Step 1 — Value inventory
Read the actual files and record each setting per environment as
current → proposed. Flag values that break the pattern across environments.
Name secrets only by their secret-store reference (e.g. a Key Vault
reference or user-secrets key), never by value. Use one column per environment
in the settings' promotion order (e.g. `LOCAL` only, or `DEV | SIT | UAT | PROD`).

| Setting | Source file | [env 1] | [env 2] | … |
|---|---|---|---|---|

Wait for approval.

## Step 2 — What deploys it
Name the pipeline, stage and template that apply each file. Check:
- Whether the pipeline trigger excludes the path (e.g. `ops/**`); if so, the
  human runs it by hand.
- Whether the deploy merges or replaces settings (e.g. a Bicep `union()` keeps
  keys that were removed, so removing a key does not roll a value back).

| File | Pipeline / stage | Triggered by the change? | Merge or replace |
|---|---|---|---|

## Step 3 — Effect on activation
State what happens when each value takes effect: a backlog processed,
functions stopping mid-run, a switch to another store, restarts. Find who else
reads the same setting or store; run the `sdlc:impact-analysis` skill if it
crosses a contract.

| Setting | Effect | Handling |
|---|---|---|

Wait for approval of Steps 2 and 3.

## Step 4 — Write one file at a time
For each file: state the existing entry used as the reference, describe the
change, wait for approval, then edit. The human runs any what-if or
validation.

## Step 5 — Rollback and post-deploy check
Per setting and environment, give the rollback value, how it is applied, and
the check that proves the change (setting visible, function state, a log line
or a query). Record pre- and post-deployment steps for the release notes
(`sdlc:release`).

| Setting | Environment | Rollback | Post-deploy check |
|---|---|---|---|

## Completion summary
- Settings changed per environment.
- Deploy route and any manual pipeline runs.
- Side effects and how they were handled.
- Rollback plan.
- Post-deploy checks as reported by the human.

Then run the `sdlc:context-update` skill for the repo.
