# Agent Instructions

This repository continues a project for creating and improving a power simulation skill library for Codex/Claude-style agents.

The current skill is MATPOWER 7.1. Future work may add more power-system calculation skills, including pandapower.

## Project Layout

- Official publishable skills live under `skills/`.
- The MATPOWER 7.1 skill lives in `skills/matpower-71/`.
- `skills/matpower-71/SKILL.md` is the English skill entry point.
- `skills/matpower-71/references/*.md` are the official English progressive references.
- Chinese translation and review materials for MATPOWER live only in `translations/matpower-71/`.
- Do not move or copy Chinese files into `skills/matpower-71/`.

## Maintenance Rules

- Keep `SKILL.md` concise; put detailed domain guidance in topic-specific reference files.
- For MATPOWER work, target MATPOWER 7.1 legacy APIs unless the user explicitly asks for newer MATPOWER 8.x MP-Core behavior.
- Preserve this skill rule: never instruct users to edit their installed MATPOWER package in place. If MATPOWER source behavior must change, tell users to copy the needed `.m` files into their current project/workspace and edit only those project-local copies.

## Update Workflow

When improving an existing skill:

1. Update the official English files under `skills/<skill-name>/` first.
2. Mirror meaningful changes into `translations/<skill-name>/` for review.
3. Keep translation files outside the official skill folder.
4. Run the skill validator if available; otherwise manually check frontmatter, required files, and reference paths.
