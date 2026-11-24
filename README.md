# RED MiniTool Skills

A collection of Agent Skills for RED MiniTool workflows. One repository can contain multiple independently installable skills.

## Layout

```text
src/
  first-skill/
    SKILL.md
    references/
    scripts/
    assets/
  second-skill/
    SKILL.md
skills.sh.json
```

The skill names above are layout examples, not published skills. Only create references, scripts, and assets when a skill needs them.

## Discover and install

After skills have been added:

```sh
npx skills add red-minitool/minitool-public-skill --list
npx skills add red-minitool/minitool-public-skill --skill your-skill-name --agent codex
```

Replace `your-skill-name` with a name shown by `--list`. The root `skills.sh.json` declares `./src` as the discovery directory. Do not add a root-level `SKILL.md`.

## Add a skill

1. Create `src/<skill-name>/SKILL.md`. Use a unique lowercase kebab-case name matching the folder.
2. Include `name` and `description` in YAML frontmatter. Describe the purpose and trigger scenarios.
3. Keep instructions focused on inputs, steps, constraints, and verification. Add supporting resources inside the same skill directory.
4. Check relative references and command examples, then verify discovery and installation in a temporary project.
5. Update this repository catalog when adding or removing a skill.

## Catalog

No skills are published yet. This repository currently contains the multi-skill layout and maintenance instructions only.
