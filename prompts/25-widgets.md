Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 16.1-16.5.

Design reference: `assets/stitch_flowday_executive_productivity_companion/home_screen_widgets/`

Widgets require native code and a development build. Confirm the EAS build setup exists before starting, and check the Expo 57 docs for the current widget approach rather than following an older tutorial.

Three widgets:

- **Right Now** — the current next action, its estimate, and a `START` button. This is the highest-value widget in the product: it answers "what should I do right now" without opening the app, which is the entire product thesis on a home screen.
- **Today** — THE THING plus secondary tasks.
- **Focus** — the active session with live remaining time.

Define one widget data contract and keep it synchronized with the Today plan. A widget showing a stale task is worse than no widget, because the user acts on it.

`START` from the widget deep-links straight into the focus session for that task. It does not open Today and make the user find the task again.

Keep the visual language identical to the app: cream background, sage accent, Hanken Grotesk, 24px radius. Widgets have their own platform constraints, so verify the fonts and colors actually render rather than assuming the app tokens carry over.

Handle the states a widget will actually spend time in: no plan yet, everything done, and not signed in. Each needs a sensible one-line message, never a blank tile or an error.

Respect the platform refresh budget. Do not attempt live-updating timers where the OS does not support them.
