# TUI Recommendations

Use this baseline for `apps/tui` or a standalone terminal application.

## Stack

- Runtime, package manager, build tool, and test runner: Bun
- UI renderer: Ink with React and TypeScript
- Linter and formatter: Biome
- Distribution: a Bun-compiled executable when users should not need a local runtime

## Recommended dependencies

Runtime:

- `ink`
- `react`

Development and tests:

- `@biomejs/biome`
- `@types/bun`
- `@types/react`
- `strip-ansi`
- `typescript`

Add argument parsers, Ink component libraries, and terminal capability packages only when the product needs them.

## Bootstrap

Initialize and install with Bun:

```sh
bun init -y
bun add ink react
bun add -d @biomejs/biome @types/bun @types/react strip-ansi typescript
```

Use a small, separable layout:

```text
src/
  cli.tsx
  app.tsx
  theme.ts
tests/
  app.test.tsx
  theme.test.ts
```

- Put only argument/environment parsing and `render(<App />)` in `src/cli.tsx`.
- Keep the root Ink component in `src/app.tsx`.
- Keep theme resolution and semantic tokens in `src/theme.ts`; make the resolver pure by passing environment values into it.
- Start `src/cli.tsx` with `#!/usr/bin/env bun` and make it executable when the package exposes a `bin` entry.
- Configure TypeScript for ESM and `jsx: react-jsx`.

Use scripts equivalent to these, replacing `<command>` with the real executable name:

```json
{
  "type": "module",
  "bin": {
    "<command>": "./src/cli.tsx"
  },
  "scripts": {
    "dev": "bun --watch ./src/cli.tsx",
    "start": "bun ./src/cli.tsx",
    "build": "bun build ./src/cli.tsx --compile --outfile ./dist/<command>",
    "test": "bun test",
    "typecheck": "tsc --noEmit",
    "check": "biome check ."
  }
}
```

## Theme awareness

Model color as semantic tokens such as `accent`, `muted`, `success`, `warning`, and `danger`. Pass the resolved theme into the component tree instead of reading environment variables inside presentation components.

Support `auto`, `light`, and `dark` modes with this precedence:

1. An explicit `--theme` option.
2. A command-specific environment variable such as `MY_APP_THEME`.
3. Best-effort terminal detection, including the background value at the end of `COLORFGBG` when present.
4. A documented dark fallback when the terminal exposes no preference.

Follow these terminal-safe rules:

- Respect `NO_COLOR` by omitting color props while retaining labels, emphasis, spacing, and selection markers.
- Prefer named ANSI colors and the terminal's default foreground/background over fixed RGB foreground/background pairs.
- Do not paint a global background by default; users expect their terminal palette to remain in control.
- Use `inverse`, bold text, borders, symbols, or labels for selection and status so color is never the only signal.
- Treat automatic detection as a hint. Allow explicit overrides and do not block startup waiting for an OSC response.
- Keep the theme resolver independent from Ink so it can be tested without a TTY.

## Ink behavior

- Render the root with Ink's `render()` API.
- Let Ink use its normal non-interactive behavior when stdout is not a TTY; do not force cursor control in pipes or CI.
- Use alternate-screen mode only for a genuinely full-screen application.
- Handle cancellation and expected errors cleanly, restore terminal state, and send diagnostics to stderr.
- Keep business logic outside Ink components so commands and state transitions can be tested deterministically.

## Tests

- Test theme precedence, invalid values, `COLORFGBG` detection, the dark fallback, and `NO_COLOR` as pure functions.
- Render representative components with Ink's `renderToString()` and remove ANSI sequences with `strip-ansi` before asserting readable content.
- Exercise light, dark, and color-disabled themes explicitly.
- Test input-driven state separately from terminal I/O where possible.
- Run `bun test`, `bun run typecheck`, and `bun run check` before handing off the project.

## Source links

- Ink: https://github.com/vadimdemedes/ink
- Ink `render()`: https://github.com/vadimdemedes/ink/blob/master/_autodocs/api-reference/render.md
- Ink `renderToString()`: https://github.com/vadimdemedes/ink/blob/master/_autodocs/api-reference/renderToString.md
- Bun TypeScript runtime: https://bun.sh/docs/runtime/typescript
- Bun standalone executables: https://bun.sh/docs/bundler/executables
- NO_COLOR: https://no-color.org/
