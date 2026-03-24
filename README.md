# Harness

Personal coding harness files for working with AI coding agents.

This repo is a small, practical collection of reusable assets I use to steer agent behavior and speed up repeated workflows. Right now it primarily contains:

- `skills/`: task-specific agent skills with `SKILL.md` instructions
- `prompts/`: reusable prompts for recurring review, design, and implementation work

## Structure

```text
.
├── AGENTS.md
├── prompts/
│   ├── end_session.md
│   └── velorah-landing-page
└── skills/
    ├── daily/
    │   └── SKILL.md
    ├── design-an-interface/
    │   └── SKILL.md
    └── write-a-skill/
        └── SKILL.md
```

## What Lives Here

### Skills

Skills are reusable instruction bundles for specific tasks. Each skill lives in its own directory and uses a `SKILL.md` file as the entry point.

Current skills include:

- `daily`: a lightweight personal news-gathering workflow
- `design-an-interface`: generates multiple competing interface designs before choosing an approach
- `write-a-skill`: guidance for creating new agent skills with clear triggers and structure

### Prompts

Prompts are standalone text assets for recurring tasks or project setups.

Current prompts include:

- `end_session.md`: a high-effort review and cleanup prompt
- `velorah-landing-page`: a detailed product/UI generation prompt for a landing page

## Conventions

- Keep reusable agent capabilities in `skills/<skill-name>/SKILL.md`
- Keep one-off or reusable freeform prompts in `prompts/`
- Prefer concise, explicit instructions over broad vague guidance
- Add supporting reference files next to a skill only when the main `SKILL.md` would become too large

## Adding New Content

Add a new skill:

1. Create `skills/<name>/`
2. Add `skills/<name>/SKILL.md`
3. Include a short description that makes the trigger conditions obvious

Add a new prompt:

1. Create a file in `prompts/`
2. Name it for the task or outcome it is meant to drive
3. Keep it directly reusable with minimal editing

## Purpose

This repo is not an application by itself. It is a working library of prompts, skills, and related harness files that support personal coding, design, and review workflows.
