# Skill repository maintenance

- Read the relevant `SKILL.md` and references before editing. Maintain each skill under `src/<name>/`; use unique lowercase kebab-case names matching the directory.
- Keep `SKILL.md` focused on scope, inputs, decisions, steps, and verification. Put task-specific details in `references/`; add scripts or assets only when needed.
- Keep CLI implementations and platform services outside this repository. Verify command examples with the installed CLI's `--help`; prefer JSON output.
- Do not include private service addresses, documentation-site routes, credentials, local workspace paths, or private repository details. Query platform knowledge through the user's installed CLI without embedding service locations.
- Update the README catalog when adding or removing skills. Update the skill's `metadata.version` and root CHANGELOG for content changes.
- Validate every `src/*/SKILL.md`, relative references, command examples, and install discovery. Format validation does not establish successful agent execution.
- Use `beta` for development and installation/update verification. Promote reviewed changes to `main` for stable distribution. Disable telemetry for development installation tests.
- Commit only reviewed public files. Do not import unrelated repository history. Preserve published branch history and avoid force pushes.
