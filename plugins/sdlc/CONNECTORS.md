# Connectors

This plugin ships no MCP servers. Skills that need a tracker or git host use
what the session already has, in the order set in `workspace-settings.md`.

| Need | Used by | First choice | Fallbacks |
|---|---|---|---|
| Fetch a ticket | `ticket-analysis` | Tracker CLI (e.g. `gh issue view`) | Tracker connector (e.g. the engineering plugin's GitHub, Atlassian or Linear server) → signed-in browser → human pastes the ticket |
| Create a PR | `raise-pr` | Git host CLI (`gh pr create`, `glab`, `az repos`) | Git host connector → human creates it from the drafted description |
| Read CI / deployments | `release`, `hotfix`, `incident-triage` | Git host CLI | Connector → human pastes the output |
| Logs, telemetry, errors | `telemetry-query` (called by `bug-fix`, `incident-triage`, `perf-investigation`) | Tools named in the settings' "Telemetry access" (e.g. `az rest` / `az monitor`) | Human runs the given query and pastes results |

Install and sign in to the CLI you name in the settings (for GitHub:
`winget install GitHub.cli`, then `gh auth login`). Systems are read-only for
`incident-triage`, `code-review` and `security-review`; anything that changes
an external system (creating a PR, pushing, commenting) waits for explicit
approval.
