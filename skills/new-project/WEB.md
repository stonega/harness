# Web Recommendations

Use this baseline for `apps/web` or a standalone web app.

## Framework

- App framework: React Router v7
- Package manager: Bun
- Linter and formatter: Biome
- Styling: Tailwind CSS v4
- UI system: shadcn/ui
- AI integration: AI SDK by Vercel
- Deployment target: Cloudflare by default

## Recommended dependencies

Core:

- `react`
- `react-dom`
- `react-router`

Code quality:

- `@biomejs/biome`

Styling:

- `tailwindcss`
- `@tailwindcss/vite` when the project uses Vite-based tooling

shadcn/ui baseline:

- `shadcn`
- `class-variance-authority`
- `clsx`
- `tailwind-merge`
- `lucide-react`
- `tw-animate-css`

AI:

- `ai`

Provider packages:

- Add only the provider packages actually needed by the project
- Keep provider selection explicit instead of installing multiple providers by default

## Notes

- Prefer Tailwind CSS v4 for all new web projects.
- Prefer Biome as the default formatter and linter for new web projects.
- Prefer initializing shadcn/ui with its CLI and selecting the React Router template when relevant.
- For new shadcn/ui projects on Tailwind v4, prefer `tw-animate-css` instead of `tailwindcss-animate`.
- Use the AI SDK only where the app genuinely needs model calls, streaming, or agent workflows.
- Do not add shadcn/ui components in bulk; add only the components the product actually uses.

## Source links

- Tailwind CSS v4: https://tailwindcss.com/blog/tailwindcss-v4
- shadcn/ui installation: https://ui.shadcn.com/docs/installation
- shadcn/ui Tailwind v4 notes: https://ui.shadcn.com/docs/tailwind-v4
- AI SDK: https://ai-sdk.dev/docs/introduction
