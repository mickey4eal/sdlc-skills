# Session Opening Prompts

Copy-paste templates for starting a Claude Code session in this workspace. They
align to the `CLAUDE.md` startup sequence, session modes, and approval
vocabulary. CC reads `CLAUDE.md` → repo `bootstrap.md` → skill automatically,
so the prompt's job is to name the **repo, mode, task, and skill**.

## Canonical skeleton
```
Repo: [repo-name]
Mode: implementation        # or: analysis — output verbosity only
Task: [one-line objective]
Skill: sdlc:[skill-name]       # the workflow: sdlc:bug-fix, sdlc:refactor, sdlc:code-review, … — omit if none applies
[Ticket: TICKET-ID]         # if a tasks/[id] folder exists
Start with the startup sequence, then propose step 1.
```

`Mode` sets how much you show (lean vs verbose). `Skill` sets what you do
(the workflow). They are independent — pick one of each. A bug-fix, review, or
spike is a **skill**, not a mode.

## Examples

### 1. Start of day / orientation (analysis)
```
Repo: [repo-name]
Mode: analysis
Task: Orient me. Read CLAUDE.md, the bootstrap, and summarise current state,
do-not-touch items, and open decisions. Do not propose code changes yet.
```

### 2. Add a feature to a feature-based repo
```
Repo: [repo-name]
Mode: implementation
Task: Add a new "ArchivePicklist" operation (PATCH /picklists/{id}/archive,
auth: standard JWT). Use an existing feature slice as the reference.
Skill: sdlc:new-feature
Ticket: 
Propose the feature definition first; wait for my approval.
```

### 3. Remove / replace a dependency
```
Repo: [repo-name]
Mode: implementation
Task: Remove package [X] and replace with [Y] across the solution.
Skill: sdlc:remove-package
Begin with the Step 1 usage audit. I will run all builds.
```

### 4. Migrate an endpoint
```
Repo: [repo-name]
Mode: implementation
Task: Migrate the remaining [controller/action] into a feature slice,
preserving the contract.
Skill: sdlc:migrate-endpoint
Ticket: TICKET-ID
Read tasks/TICKET-ID first, then produce the Step 1 scope statement.
```

### 5. Update package versions
```
Repo: [repo-name]
Mode: implementation
Task: Update [packages] to [target versions].
Skill: sdlc:nuget-update
Produce the audit + breaking-change assessment first. I run builds between each.
```

### 6. Behaviour-preserving refactor
```
Repo: [repo-name]
Mode: implementation
Task: Reorganise [module] into [target structure] with no behaviour change.
Skill: sdlc:refactor
Capture the baseline (Step 1) before proposing any move.
```

### 7. Add infrastructure / cross-cutting concern
```
Repo: [repo-name]
Mode: implementation
Task: Add [health checks / telemetry / resilience policy] at the host.
Reference: [reference repo or existing infra file].
Skill: sdlc:add-infrastructure
Read the reference first and state it before proposing.
```

### 8. Close out a ticket (context refresh)
```
Repo: [repo-name]
Mode: implementation
Task: Ticket [TICKET-ID] code work is complete and verified. Run the
sdlc:context-update skill to refresh all affected context artefacts.
Skill: sdlc:context-update
Walk the file list one at a time; bootstrap.md last.
```

### 9. Build a missing repo's context (a skipped repo)
```
Repo: [repo-name]
Mode: analysis
Task: This repo has no ai-context yet. Run sdlc:context-first to build it,
then propose the standard context file set for my approval. No code changes.
Skill: sdlc:context-first
```

### 10. Fix a bug
```
Repo: [repo-name]
Mode: implementation
Task: Fix [defect]. Observed: [X]. Expected: [Y]. Repro: [steps]. Error: [msg].
Skill: sdlc:bug-fix
Ticket: 
Reproduce it first (Step 1); do not propose a fix until the root cause is agreed.
```

### 11. Review a change before merge
```
Repo: [repo-name]
Mode: analysis
Task: Review [branch/PR/diff]. Intended change: [what it should do]. Contract to
preserve: [routes/schema/auth].
Skill: sdlc:code-review
Read-only. Produce the findings table; I decide what to fix.
```

### 12. Add tests to existing code
```
Repo: [repo-name]
Mode: implementation
Task: Add coverage for [module]. Goal: [characterise current behaviour / cover
path X]. Current behaviour is the source of truth.
Skill: sdlc:test-authoring
Map coverage and gaps first (Step 1); do not change the code under test.
```

### 13. Time-boxed spike / investigation
```
Repo: [repo-name]
Mode: analysis
Task: Investigate [question]. Feeds decision: [decision]. Time box: [effort].
Done when: [criteria].
Skill: sdlc:spike
No production code. Prototype only in scratch\. End with a recommendation.
```

### 14. Start a new ticket (analysis)
```
Repo: [repo-name or "multiple"]
Mode: analysis
Task: Analyse this ticket before any code. Fetch it from Jira.
Skill: sdlc:ticket-analysis
Ticket: [TICKET-ID]
Snapshot the ticket (Step 0), then restate the ACs (Step 1) and wait for my
approval.
```

### 15. Raise a pull request
```
Repo: [repo-name]
Mode: implementation
Task: Ticket [TICKET-ID] is ready for review. Target: [main / release/x.y.z].
Skill: sdlc:raise-pr
Run the branch and scope checks, then draft the description. Do not push or
create the PR until I approve.
```

### 16. Prepare a release
```
Repo: multiple — [repo list]
Mode: analysis
Task: Prepare release [x.y.z] containing [ticket list]. Environments: DEV → SIT
→ UAT → PROD.
Skill: sdlc:release
Start with the release scope check per repo. No merges without approval.
```

### 17. Production hotfix
```
Repo: [repo-name]
Mode: implementation
Task: PROD defect: [symptom, impact]. Deployed version: [release/x.y.z or commit].
Skill: sdlc:hotfix
Ticket: [TICKET-ID]
Confirm the deployed baseline (Step 1) before creating any branch.
```

### 18. Triage a live issue
```
Repo: [suspected repo or "unknown"]
Mode: analysis
Task: [Environment] issue: [symptom]. Started: [time]. Recent deploys: [list].
Evidence: [paste errors / logs / dead-letter counts].
Skill: sdlc:incident-triage
Read-only. Frame the incident and propose a severity first.
```

### 19. Record a design decision
```
Repo: [repo-name or "global"]
Mode: implementation
Task: Record the decision: [decision]. Ratified by [who] on [date]. Options
considered: [list].
Skill: sdlc:adr
Ticket: [TICKET-ID]
Propose the location and number first.
```

### 20. Security review
```
Repo: [repo-name]
Mode: analysis
Task: Security review of [branch / PR / path]. Exposure: [internet-facing /
internal / behind an API gateway].
Skill: sdlc:security-review
Read-only. Every finding needs an exploit scenario; I decide what to fix.
```

### 21. Performance problem
```
Repo: [repo-name]
Mode: analysis
Task: [Endpoint / query / page] takes [observed]; acceptable is [target].
Repro: [request, data volume, environment].
Skill: sdlc:perf-investigation
Agree the baseline measurement (Step 1) before proposing any change.
```

### 22. Hand over / pause a ticket
```
Repo: [repo-name]
Mode: implementation
Task: Pause [TICKET-ID] here. Write the handoff for [a fresh session /
colleague name].
Skill: sdlc:handover
Ticket: [TICKET-ID]
Include a ready-to-paste resume prompt.
```

### 23. Weekly skill retro
```
Repo: workspace
Mode: analysis
Task: Run the skill retro since [last retro date]. Extra lessons: [any not yet
in skill-feedback.md].
Skill: sdlc:skill-retro
Gather and triage first (Steps 1–2); wait for my approval before proposing edits.
```

## Typical ticket flow
`sdlc:ticket-analysis` → build skill (`sdlc:new-feature`,
`sdlc:bug-fix`, …) →
`sdlc:code-review` / `sdlc:security-review` → `sdlc:context-update` → `sdlc:raise-pr` →
`sdlc:release`. Use `sdlc:handover` whenever a session ends mid-ticket.

## Tips
- Mode defaults to `implementation` if omitted — state `Mode: analysis` for full
  tables/audit trails.
- `Mode` = verbosity; `Skill` = workflow. sdlc:bug-fix, sdlc:code-review,
  sdlc:test-authoring, and sdlc:spike are skills — combine them with either mode.
- `Repo:` can be "multiple" for cross-repo skills (sdlc:ticket-analysis,
  sdlc:release) — list the repos in the task.
- A skill can also be invoked directly as a slash command, e.g. `/sdlc:bug-fix`.
- Respond to proposals with the approval vocabulary: `Approved. Proceed.` /
  `Approved with amendments: …. Proceed.` / `Not approved. …. Revise.` /
  `Observation noted. Continue.` / `Skip. Continue.`
- You run all builds/tests; paste the output and CC interprets it.
