# prompts/AGENTS.md

This file governs **how prompts in this folder are executed**. It does not replace the product
spec.

The product spec is `docs/AGENTS.md`. Read it before every prompt. It is the source of truth for
product philosophy, tech stack, architecture, naming, security, styling, and the release quality
bar.

The roadmap is `docs/todo.md`. Every prompt in this folder maps to one or more numbered tasks in
that roadmap.

---

## Repository reality (read this before assuming anything)

Flowday ships as a **single Expo app at `flowstate/`**, managed with npm, using Expo API routes for
backend endpoints. This is a deliberate decision recorded in `docs/todo.md` Task 0.4, not an
accident. The Turborepo monorepo in `docs/AGENTS.md` § Architecture is the eventual target, revisited
when the Next.js web app begins at Phase 19.

Do not introduce pnpm, workspaces, `apps/`, or `packages/`. Do not restructure the repository as a
side effect of a feature prompt. Module boundaries still matter: planning, AI, and validation logic
stay in their own modules so they can move later without a rewrite.

Current stack, verified from `flowstate/package.json`:

- Expo SDK 57 (`expo ~57.0.14`), React Native 0.86, React 19.2
- Expo Router 57 with `typedRoutes` and `reactCompiler` enabled
- NativeWind **v5 preview** (`nativewind ^5.0.0-preview.4`) + Tailwind v4 + `react-native-css`
- `@expo-google-fonts/hanken-grotesk`, loaded via the `expo-font` config plugin
- `expo-image`, `expo-symbols`, `expo-glass-effect`, `@expo/ui`
- `react-native-reanimated` 4.5.1 + `react-native-worklets`

Not installed yet, and therefore not available until a prompt explicitly adds them: Clerk,
Supabase, TanStack Query, Zustand, AsyncStorage, PostHog, Sentry, RevenueCat, expo-notifications,
expo-calendar.

Source layout in use:

```txt
flowstate/src/
  app/          # Expo Router routes only
  screens/      # screen implementations, one folder per screen
  components/   # shared UI primitives
  theme/        # design tokens (colors, fonts, typography, spacing, radius, shadows, motion)
  constants/    # images.ts and other centralized constants
  types/
```

Route files stay thin. See `docs/AGENTS.md` § `app/` Rules.

---

## Expo 57 rule

Expo has changed significantly. Before writing code that touches Expo APIs, routing, config
plugins, or native modules, check the versioned docs at
<https://docs.expo.dev/versions/v57.0.0/>. Do not use patterns from Expo SDK 50-53 tutorials.

The same applies to NativeWind: this project is on the **v5 preview**, not v4. Use
<https://www.nativewind.dev/v5/> only.

---

## Design reference rule

All screen designs come from Google Stitch and live in:

```txt
assets/stitch_flowday_executive_productivity_companion/<screen_name>/
  screen.png    # the visual reference
  code.html     # the generated markup, useful for exact spacing/hierarchy
```

The design system is `assets/stitch_flowday_executive_productivity_companion/flowday_adaptive_harmony/DESIGN.md`.

When a prompt names a screen folder:

1. Read `screen.png` for layout and hierarchy.
2. Read `code.html` when you need exact spacing, ordering, or copy.
3. Match it as closely as React Native allows.
4. Do not redesign it. Do not add sections that are not in the reference.
5. If the reference contradicts `docs/AGENTS.md`, follow `docs/AGENTS.md` and say what you changed
   and why.

**Canonical variants.** Several screens have multiple generated variants. Use only the ones listed
in `prompts/SCREEN-MAP.md`. The rejected variants use a 5-tab bar with a separate `AI` tab; this
project uses the 4-tab bar (Today, Tasks, Focus, Insights) with the AI companion reachable from a
floating action button. Never mix the two families.

---

## Naming

The product is **Flowday**, everywhere, with no exceptions.

The `app.json` name, slug, and scheme, the `package.json` name, and the onboarding copy currently
say `flowstate`. Prompt `00` renames them. Once that prompt has run, "flowstate" should appear
nowhere except possibly the directory name, which is not user-visible.

Renaming the Expo `scheme` invalidates deep links and OAuth redirect URIs, so it must happen before
Clerk is configured in prompt `07`.

---

## How to execute a prompt in this folder

1. Read `docs/AGENTS.md`.
2. Read the roadmap tasks the prompt references in `docs/todo.md`.
3. Read the named design references.
4. Search the existing code for components, hooks, tokens, and helpers you can reuse. Reuse beats
   new files.
5. Implement the smallest version that fully satisfies the prompt.
6. Handle loading, error, and empty states. Every one of them.
7. Run `npx tsc --noEmit` and `npm run lint` inside `flowstate/`. Fix what you broke.
8. Report: what changed, the one architectural decision worth knowing, how to test it, what is
   still missing.

---

## Hard guardrails for every prompt

- Do not add a dependency without saying what problem it solves and getting approval first.
- Do not upgrade NativeWind, Expo, React Native, or Tailwind.
- Do not put secrets in the Expo app. AI provider keys, Supabase service keys, and Clerk secret
  keys are server-side only.
- Do not let a screen call an LLM directly. Calls go through the server AI gateway.
- Do not persist AI output without schema validation.
- Do not modify screens the prompt did not mention.
- Do not use guilt language. See `docs/AGENTS.md` § No guilt-based UX.
- Do not communicate meaning through color alone. Every energy state, priority tier, and status
  needs a text or icon signal too.
- Ask before implementing if the prompt is ambiguous in a way that would cause rework.
