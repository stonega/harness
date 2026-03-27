# Mobile Recommendations

Use this baseline for `apps/mobile` or a standalone mobile app.

## Framework

- App framework: Expo
- Package manager: Bun
- Linter and formatter: Biome
- Styling: Uniwind
- Tailwind version: Tailwind CSS v4

## Recommended dependencies

Core:

- `expo`
- `react`
- `react-native`

Code quality:

- `@biomejs/biome`

Styling baseline:

- `uniwind`
- `tailwindcss`

Data fetching:

- `@tanstack/react-query`

When the chosen Uniwind tier requires native support:

- `react-native-reanimated`
- `react-native-worklets`
- `react-native-nitro-modules` when required by the selected Uniwind version

## Notes

- Prefer Expo for all new mobile projects in this harness.
- Prefer Biome as the default formatter and linter for new mobile projects.
- Prefer TanStack Query as the default data fetching and server-state package for new mobile projects.
- Uniwind only supports Tailwind CSS v4.
- Keep `global.css` at the project root unless there is a strong reason not to.
- Import `global.css` from the main app component or root layout, not the root registration file.
- If the selected Uniwind setup requires native modules, plan for a native rebuild and do not assume Expo Go support.
- Check compatibility for the chosen Expo SDK and React Native version before locking versions.

## Source links

- TanStack Query React Native guide: https://tanstack.com/query/latest/docs/framework/react/react-native
- Uniwind introduction: https://docs.uniwind.dev/
- Uniwind quickstart: https://docs.uniwind.dev/quickstart
- Uniwind FAQ: https://docs.uniwind.dev/faq
- Uniwind compatibility: https://docs.uniwind.dev/pro/compatibility
