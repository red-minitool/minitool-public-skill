# RED MiniTool Skills

A collection of Agent Skills for RED MiniTool workflows. Each skill is independently installable from `src/<skill-name>/`.

## Beta development

This branch publishes development content for install and update verification. The `main` branch remains the stable entry point; changes on `beta` are not automatically promoted.

Use Node.js 22.20.0 or newer and Git. Run these commands in a temporary project for testing:

```sh
DISABLE_TELEMETRY=1 npx skills@1.8.1 add 'red-minitool/minitool-public-skill#beta' --list
DISABLE_TELEMETRY=1 npx skills@1.8.1 add 'red-minitool/minitool-public-skill#beta' --skill minitool-h5-builder --agent codex
DISABLE_TELEMETRY=1 npx skills@1.8.1 check minitool-h5-builder --project
DISABLE_TELEMETRY=1 npx skills@1.8.1 update minitool-h5-builder --project -y
DISABLE_TELEMETRY=1 npx skills@1.8.1 remove minitool-h5-builder -y
```

`check` and `update` follow the Git source and branch recorded in `skills-lock.json`. A beta installation continues following `beta`; installing from `main` is an explicit source change. Review local edits before reinstalling. Removing a named skill removes its installations across project agents; omit global flags to keep these commands project-scoped.

Installing a skill supplies instructions and resources to an agent. Platform CLI operations additionally require a compatible installed `redminitool` and the user's authorization.

## Catalog

| Skill | Version | Purpose |
| --- | --- | --- |
| [minitool-h5-builder](src/minitool-h5-builder/SKILL.md) | 0.2.4-beta.1 | Build or adapt offline H5 content and verify its ZIP artifact |

## Layout and maintenance

Each skill contains a required `SKILL.md` and only the references, scripts, or assets it actually needs. Add future skills beside the existing directory under `src/`, not inside another skill. The root `skills.sh.json` declares `./src` as the discovery directory; do not add a root-level `SKILL.md`.

Use a unique `name` matching the skill directory, describe purpose and triggers in `description`, and keep relative references inside the skill. Update the catalog when adding a skill. See [AGENTS.md](AGENTS.md) and [CHANGELOG.md](CHANGELOG.md) for maintenance rules and versions.

## Validation

```sh
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements-dev.txt
for skill in src/*/SKILL.md; do
  test -f "$skill" || exit 1
  .venv/bin/skills-ref validate "$(dirname "$skill")" || exit 1
done
```

CI validates all skills under `src/`. Also check relative links and actual task behavior when changing a skill's workflow.
