# sdlc-skills

A Claude Code plugin marketplace with one plugin, [`sdlc`](plugins/sdlc/README.md):
gated SDLC workflows (ticket analysis, bug fixes, features, refactors,
reviews, releases, hotfixes, handovers) for a context-first engineering
workspace.

```
/plugin marketplace add mickey4eal/sdlc-skills
/plugin install sdlc@sdlc-skills
/sdlc:setup-workspace
```

## Layout

```
.claude-plugin/marketplace.json   # marketplace manifest
plugins/sdlc/
  .claude-plugin/plugin.json      # plugin manifest (version lives here)
  README.md  CONNECTORS.md  CHANGELOG.md
  skills/<skill>/SKILL.md         # 26 skills
  skills/setup-workspace/templates/
```

## Changing a skill
Edit it here, add a `CHANGELOG.md` entry, bump `version` in `plugin.json`,
commit and push, then update the installed plugin from `/plugin`.
`sdlc:skill-retro` follows the same route.
