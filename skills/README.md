# Skills

This directory contains the project-local skills maintained in this repo.

It also tracks a few skills installed from external sources so there is one place to see:

- What is available locally in this repo
- Which skills are installed globally from outside sources
- How to reinstall those external skills if needed

## Local Skills

These skills live under [`skills/`](./) and are versioned with this repository.

| Skill | Purpose | Path |
| --- | --- | --- |
| `daily` | Personal daily-news workflow. The current draft is lightweight and should likely be expanded before relying on it. | [`skills/daily/`](./daily/) |
| `design-an-interface` | Generate multiple interface designs and compare them before choosing one. | [`skills/design-an-interface/`](./design-an-interface/) |
| `new-project` | Bootstrap a new project from this harness with repo layout, `AGENTS.md`, starter docs, and the default stack. | [`skills/new-project/`](./new-project/) |
| `write-a-skill` | Draft new skills with a clearer structure, references, and progressive disclosure. | [`skills/write-a-skill/`](./write-a-skill/) |

## External Skills

These are not stored in this repo. They are installed globally from third-party repositories.

### `skill-creator`

Source: `https://github.com/anthropics/skills`

Purpose:
- Create new skills
- Improve existing skills
- Run evals and benchmark iterations

Install command:

```bash
npx skills add https://github.com/anthropics/skills --skill skill-creator
```

### `obsidian-skills`

Source: `https://github.com/kepano/obsidian-skills.git`

Purpose:
- Add Obsidian-specific workflows and helpers
- Support note, vault, and plugin-related tasks depending on the installed skill set

Install command:

```bash
npx skills add https://github.com/kepano/obsidian-skills.git
```

## Maintenance Notes

- Keep local skills documented here when adding, renaming, or removing them.
- Keep external skills listed with both their source repo and reinstall command.
- Prefer clear skill names and short purpose summaries so this file works as an index, not just a scratchpad.
