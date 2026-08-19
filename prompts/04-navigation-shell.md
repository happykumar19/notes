Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 1.2, 1.4, 1.9, 1.10, and 1.11.

Design reference for the tab bar: `assets/stitch_flowday_executive_productivity_companion/today/screen.png` and its `code.html`.

Build the Expo Router navigation shell with placeholder screens only. No feature UI in this prompt.

Route groups:

```txt
src/app/
  (onboarding)/    welcome, purpose, challenges, structure, peak-focus, ready
  (auth)/          sign-in, sign-up
  (tabs)/          today, tasks, focus, insights
  brain-dump/      index, review
  task/[id]/       index, breakdown
  focus/           session, complete
  stuck/           index, [reason]
  rescue/
  schedule/
  reflection/
  settings/
  companion/
```

Tab bar: exactly four tabs — Today, Tasks, Focus, Insights. The AI companion is **not** a tab; it opens from a floating action button anchored bottom-right, per `DESIGN.md` and `ai_companion_1`. Do not build the five-tab variant that appears in some generated screens.

Presentation:

- Everything outside `(tabs)` presents as a modal with a back affordance, except the focus session.
- The focus session is full-screen with no tab bar and no header, because `docs/AGENTS.md` § Focus Mode Rules requires minimum cognitive noise.

Also establish the screen conventions from Task 1.4 so no screen reinvents them: a safe-area screen container, keyboard avoidance and dismissal, scroll behavior, and the shared loading and error slots.

Routing rules, using placeholder state for now since Clerk does not exist yet:

- Unauthenticated goes to `(onboarding)`.
- Authenticated but onboarding incomplete goes to `(onboarding)`.
- Authenticated and onboarded goes to `(tabs)/today`.

Keep `typedRoutes` passing. Verify state survives navigating away and back, and verify Android hardware back behaves correctly on every modal.
