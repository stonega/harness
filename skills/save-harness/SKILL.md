---
name: save-harness
description: Save reusable harness assets into this repository. Use whenever the user wants a skill, prompt, or other reusable agent instruction saved into the local harness repo, especially when they say to save it "to the harness", "as a skill", or "as a prompt".
---

# Save Harness

## Purpose

Route reusable agent assets into the correct location in this repository.

Current harness root: `~/Documents/harness`

## Save rules

1. Save reusable skills under `skills/<skill-name>/SKILL.md`.
2. Save reusable prompts under `prompts/<prompt-name>`.
3. Keep skill names and prompt names short, descriptive, and filesystem-safe.
4. Prefer updating an existing skill or prompt when the user is clearly refining one that already exists.

## Skill format

- Start with YAML frontmatter containing `name` and `description`.
- Keep the body concise and action-oriented.
- Use relative links to adjacent reference files only when they are actually needed.
- Do not create extra files for a simple skill unless the instructions would be clearer split out.

## Prompt format

- Save the prompt as plain text or Markdown, whichever best matches the content.
- Keep the prompt directly usable without extra wrapper explanation.
- If the user gives a title, use that as the filename when practical.

## Working rule

When the user asks to save something into the harness and the target is not otherwise specified:

- A reusable workflow or standing instruction belongs in `skills/`.
- A reusable one-off drafting or review instruction belongs in `prompts/`.

If both are requested, save both in their respective directories.
