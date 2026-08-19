Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Task 1.3.

NativeWind is already installed and configured in `flowstate/` at version `5.0.0-preview.4` with Tailwind v4 and `react-native-css`. This prompt verifies that setup rather than redoing it.

Confirm, and fix only what is actually broken:

- `global.css` is imported from the root layout and Tailwind utilities render on both iOS and Android.
- `metro.config.js` and `postcss.config.mjs` match the NativeWind v5 preview setup.
- `nativewind-env.d.ts` gives `className` correct TypeScript coverage on React Native components.
- The `lightningcss` override in `package.json` is still required; if it is a workaround for a fixed bug, say so rather than removing it silently.

Use only <https://www.nativewind.dev/v5/> as documentation. Do not apply v4 setup steps. Do not upgrade NativeWind, Tailwind, or Expo.

If everything already works, say so and change nothing.
