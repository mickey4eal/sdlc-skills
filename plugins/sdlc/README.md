# sdlc — gated SDLC workflows for Claude Code

A Claude Code plugin of 28 skills that run engineering work through a fixed,
approval-gated workflow: understand the repo first, propose before writing, one
step at a time, the human runs builds and tests, and context docs are kept up
to date after every ticket.

Skills are invoked as `/sdlc:<skill>`, e.g. `/sdlc:bug-fix`, and Claude also
picks them automatically from their descriptions.

## Install

From this repo's marketplace (local clone or GitHub):

```
/plugin marketplace add mickey4eal/sdlc-skills
/plugin install sdlc@sdlc-skills
```

For a local clone, pass its path instead:
`/plugin marketplace add "C:\path\to\sdlc-skills"`.

## Set up the workspace

The skills read and write a workspace folder — global rules (`CLAUDE.md`),
settings, per-repo context, task folders and a feedback inbox. Run:

```
/sdlc:setup-workspace
```

It creates the folders from the templates in
`skills/setup-workspace/templates/`, writes the pointer file
`.claude/sdlc.md` (user-level `~/.claude/sdlc.md` or per project), and walks
through `workspace-settings.md`.

Every skill resolves `[workspace]` from `.claude/sdlc.md` — the current
project, then its parent folders, then `~/.claude/sdlc.md` — and stops with an
offer to run `/sdlc:setup-workspace` if none is found.

## Skills

| Group | Skill | What it does |
|---|---|---|
| Setup | `setup-workspace` | Scaffold the workspace and pointer file |
| Background | `engineering-standards` | Long-form engineering rules (not user-invocable) |
| Context | `context-first` | Pre-code understanding pass for a repo |
| | `context-update` | Refresh ai-context after a ticket |
| Planning | `ticket-analysis` | Ticket → analysed task folder and plan |
| | `impact-analysis` | Consumers of a changing contract, breaking changes, rollout order |
| | `spike` | Time-boxed investigation, no production code |
| | `adr` | Record an architecture decision |
| Change | `bug-fix` | Minimal fix with a failing-first regression test |
| | `hotfix` | Urgent production fix, merged back everywhere |
| | `new-feature` | New vertical-slice feature from a reference |
| | `add-infrastructure` | Cross-cutting concern wired at the entry point |
| | `migrate-endpoint` | Change an endpoint's pattern, keep its contract |
| | `refactor` | Behaviour-preserving structural change |
| | `drop-startup` | Startup.cs → modern host builder |
| | `nuget-update` | Package updates, one per build gate |
| | `remove-package` | Replace and remove a dependency |
| | `test-authoring` | Tests around existing code |
| | `config-change` | Config-only change: per-environment values, deploy route, rollback by value |
| Review | `code-review` | Read-only review with ranked findings |
| | `security-review` | Read-only security review |
| | `perf-investigation` | Measurement-first performance fix |
| Delivery | `raise-pr` | Scope checks and PR description; creates it only if the working agreement allows |
| | `release` | Release notes, deployment order, checklist |
| | `env-verification` | Verify a delivered ticket in an environment; human runs the cases |
| | `incident-triage` | Read-only evidence, severity and decision |
| | `telemetry-query` | Read-only telemetry/log investigation; evidence timeline for the calling skill |
| | `handover` | Handoff document for a fresh session |
| Improvement | `skill-retro` | Apply reviewed lessons from the feedback inbox |

## Workspace rules the skills assume

`CLAUDE.md` from the template defines them; keep them if you edit it:
- Approval vocabulary (`Approved. Proceed.`, `Not approved. … Revise.`, …).
- One step at a time; nothing is written before approval.
- The human runs builds and tests and pastes the output.
- Branches, commits, pushes and PRs follow the "Working agreement" in
  `workspace-settings.md`; by default the human does all four.

See [CONNECTORS.md](CONNECTORS.md) for tracker and git host access, and
[CHANGELOG.md](CHANGELOG.md) for versions.
