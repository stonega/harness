# Skills

This directory contains the project-local skills maintained in this repo.

It also tracks a few skills installed from external sources so there is one place to see:

- What is available locally in this repo
- Which skills are installed globally from outside sources
- How to reinstall those external skills if needed

## Local Skills

These skills live under [`skills/`](./) and are versioned with this repository.

Repository source: `https://github.com/stonega/harness`

### `daily`

Purpose:
- Personal daily-news workflow
- Current draft is lightweight and should likely be expanded before relying on it

Path: [`skills/daily/`](./daily/)

Install command:

```bash
npx skills add https://github.com/stonega/harness --skill daily
```

### `design-an-interface`

Purpose:
- Generate multiple interface designs before committing to one
- Compare alternative module or API shapes

Path: [`skills/design-an-interface/`](./design-an-interface/)

Install command:

```bash
npx skills add https://github.com/stonega/harness --skill design-an-interface
```

### `new-project`

Purpose:
- Bootstrap a new project from this harness
- Set up repo layout, `AGENTS.md`, starter docs, and the default stack

Path: [`skills/new-project/`](./new-project/)

Install command:

```bash
npx skills add https://github.com/stonega/harness --skill new-project
```

### `write-a-skill`

Purpose:
- Draft new skills with clearer structure
- Encourage references and progressive disclosure

Path: [`skills/write-a-skill/`](./write-a-skill/)

Install command:

```bash
npx skills add https://github.com/stonega/harness --skill write-a-skill
```

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
