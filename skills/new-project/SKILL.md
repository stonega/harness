---
name: new-project
description: Bootstrap a new project from this harness, including repo layout, project-specific AGENTS instructions, starter documentation, and the default app stack. Use whenever the user wants to create, scaffold, initialize, or structure a new repo, especially when they mention Bun, Expo, React Router v7, Hono, Cloudflare, Ink, monorepos, standalone apps, or "start a new project" without a fully specified setup.
---

# New Project

## Quick start

Turn a rough product request into an initialized repo with a justified layout, a project-specific `AGENTS.md`, starter docs, a minimal default stack, and Biome as the default linter/formatter unless the user asked for alternatives.

Defaults: Bun for package management and workspaces, Biome for linting and formatting, React Router v7 for web, Expo for mobile, Hono for backend, Cloudflare for deployment, and Ink for TUI.

Read [WEB.md](WEB.md) only for web apps. Read [MOBILE.md](MOBILE.md) only for mobile apps.

## Workflow

1. Infer the smallest viable shape.
   - Prefer standalone for one deployable surface.
   - Prefer a monorepo for multiple apps, shared packages, or separate deployment targets.
   - If the request is vague, choose the smallest structure that will not block the likely roadmap.
2. Build `AGENTS.md` before the rest of the repo.
   - First fetch `https://raw.githubusercontent.com/stonega/harness/refs/heads/main/_AGENTS.md`.
   - Adapt it to the actual project structure, commands, and testing workflow.
   - If the remote file is unavailable, use the local harness copy and say so.
3. Create the base structure.
   - Always create `README.md`, `AGENTS.md`, `docs/design`, `docs/implementation`, `docs/user`, `scripts`, `examples`, and `postmortem`.
   - For monorepos, use `apps/` for deployable apps and `packages/` for shared code.
4. Initialize Bun first, add Biome, and keep dependencies explicit.
5. Choose the scaffold by surface.
   - Web: React Router v7.
   - Backend: Hono on Cloudflare.
   - Mobile: Expo.
   - Web + backend: separate apps in one repo.
   - Web + mobile + backend: `apps/web`, `apps/mobile`, `apps/backend`.
   - TUI: add Ink only when the user actually needs it.
6. Write docs immediately.
   - Add `docs/design/architecture.md` with structure and rationale.
   - Add `docs/implementation/setup.md` with bootstrap and local-dev notes.
   - Update `README.md` with install, run, and test commands.
7. Add baseline quality support.
   - Add deterministic tests for generated logic or scripts.
   - Add idempotent scripts when they simplify setup or repeated workflows.
   - Add examples when a new workflow or API is introduced.
8. Apply stack-specific defaults only where needed.
   - Use [WEB.md](WEB.md) for web packages and setup.
   - Use [MOBILE.md](MOBILE.md) for mobile packages and setup.

## Expected output

Leave the repo with a coherent layout, a project-specific `AGENTS.md`, starter docs, a runnable README, and minimal dependencies.

## Default layouts

Standalone: `AGENTS.md`, `README.md`, `docs/`, `src/`, `tests/`, `scripts/`, `examples/`, `postmortem/`
Monorepo: `AGENTS.md`, `README.md`, `apps/`, `packages/`, `docs/`, `scripts/`, `examples/`, `postmortem/`

## Rules

- Prefer standalone unless multiple independently deployed surfaces are clearly needed.
- Keep generated `AGENTS.md` aligned with the actual repo structure.
- Create documentation and tests as part of setup, not as a later cleanup pass.
