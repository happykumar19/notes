Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 0.6-0.9.

Two parts: audit the existing app, then rename it.

**Part 1 — Audit.** Report, without changing behavior:

1. Which roadmap tasks in `docs/todo.md` are already satisfied by existing code, and where that code lives.
2. Which design tokens in `flowstate/src/theme/` diverge from `assets/stitch_flowday_executive_productivity_companion/flowday_adaptive_harmony/DESIGN.md`, including shadow, radius, and elevation values.
3. Whether strict TypeScript is on, whether `typedRoutes` resolves, and what `npm run lint` and `npx tsc --noEmit` currently report.

Then finish Tasks 0.6 to 0.8: strict TypeScript, path aliases, working `lint`, `typecheck`, and `test` scripts.

**Part 2 — Rename to Flowday.** The product is Flowday. Rename every occurrence of "flowstate":

- `app.json`: `name`, `slug`, and `scheme`
- `package.json`: `name`
- The onboarding screen's visible logo text
- Any comment, type name, or identifier carrying the old name

Changing the Expo `scheme` breaks deep links and OAuth redirect URIs, which is exactly why this runs before Clerk is configured in prompt `07`. After renaming, verify the app still launches on both platforms and that deep links resolve under the new scheme.

The `flowstate/` directory name is not user-visible. Renaming it changes every import path and tooling reference, so leave it unless you can do it cleanly in one pass; say which you chose.

Do not restructure the repository. The single-app layout is the decision recorded in `docs/todo.md` Task 0.4.
