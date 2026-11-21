# Workspace Settings

Workplace-specific values read by the workspace skills. Skills stay generic and
take their tracker, git, environment, hosting and security details from here.

## Issue tracker
- Tracker: [Jira / Linear / GitHub Issues / …]
- Site: `[https://your-site.example]`
- Ticket URL: `[https://your-site.example/browse/[ticket-id]]`
- Ticket key prefixes: `[PROJ]` (e.g. `[PROJ-1234]`).
- Fetch order: [tracker connector] → [signed-in browser] → ask the human to paste.

## Source control
- Host: [GitHub / GitLab / Azure DevOps], organisation `[org]`.
- CLI: `[gh / glab / az]`.
- Default branch: `[main]`.
- Release branches: `[release/x.y.z]`. Exceptions: [repo → pattern].
- Feature branches: `[feature/[ticket-id]-[desc]]`.
- Hotfix branches: `[hotfix/[ticket-id]-[desc]]`.
- PR title: `[[ticket-id]: [imperative summary]]`.
- Who creates branches: [the human / Claude on approval].

## Environments and releases
- Promotion order: [DEV → TEST → STAGING → PROD].
- Pipelines: [where pipeline definitions live].
- Release notes location: `[workspace]\tasks\[ticket-id]\release-notes-x.y.z.md`.
- Release notes format reference: [most recent release notes file].
- Release notes sections: Summary; Pre-Deployment Steps (per environment);
  Post Deployment Steps; Known Issues and How to Resolve; Notes (rollback
  position).
- Usual deployment order: [e.g. database before the APIs that need new objects;
  producers of new message fields before consumers; consumers before removals].

## Hosting and observability
- Hosting: [platform and plans; pointer to a hosting reference file].
- Messaging: [broker, topics, dead-letter handling].
- Telemetry and errors: [APM / logging / error tracking tools].
- Data stores: [databases, search, storage].
- Known incident hazards: [recurring causes worth checking first].

## Security stack
- Authentication: [identity provider, token type].
- Authorisation: [permission mechanism].
- Secrets: [secret store, identity; credential files never committed].
- Gateway: [API gateway, if any].
- Data access: [ORM / micro-ORM; parameterisation rules].
- Vulnerability checks: [commands, e.g. `dotnet list package --vulnerable`, `npm audit`].

## Consumer paths
Where consumers of each contract type live, read by `sdlc:impact-analysis`.
- HTTP APIs: [repos / apps / external clients that call them].
- Client libraries and shared packages: [repos that reference them].
- Messages / events: [subscribers per topic or queue].
- Database objects: [repos, reports, jobs that read or write them].
- Search indexes: [repos that query them].
- Configuration keys: [where shared configuration is consumed].

## Performance tooling
- Backend: [runtime, data access, telemetry, database measurement commands].
- Search: [profiling approach].
- Frontend: [framework, browser tooling].
