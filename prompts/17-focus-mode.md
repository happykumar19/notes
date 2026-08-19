Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 9.1-9.8.

Design references:

```txt
assets/stitch_flowday_executive_productivity_companion/focus/                    # primary
assets/stitch_flowday_executive_productivity_companion/focus_animated_deep_flow/ # motion reference, same layout
assets/stitch_flowday_executive_productivity_companion/focus_mode_completed/     # completion
```

Use the dark full-screen `focus` design: no tab bar, no header, no chrome. This is the one screen in the product that takes over completely, per `docs/AGENTS.md` § Focus Mode Rules.

Two deliberate deviations from the reference, both required by `docs/AGENTS.md`:

- Add an `I'm Stuck` secondary action, which `focus` omits. Take its placement from `focus_mode_active_2` without adopting that screen's tab bar.
- Add the `Next Micro-Step` row that shows the current breakdown step, also from `focus_mode_active_2`.

The screen shows exactly this and nothing else: current task, next action, ring timer, pause, complete, I'm stuck. No charts, no statistics, no task list, no notifications.

Data: a `focus_sessions` migration storing task, planned duration, actual duration, start, end, and completion state. Active timer state lives in Zustand.

The timer is the part that will break if rushed. Elapsed time is computed from timestamps, never accumulated from intervals or frames. It must stay correct across backgrounding, screen lock, a phone call, and a device clock change. Test all four before calling this done.

Pause and resume persist, so force-quitting mid-session does not lose the work.

Completion writes actual duration next to the estimate, feeding the estimation-bias tracking in `docs/AGENTS.md` § Time Estimation. Deviation is recorded as data and never surfaced as criticism. Then optionally advance the parent task or its current step.

Wire `Start Focus` from Today and task detail into this route, and wire `I'm Stuck` to the flow from prompt `16` with session context preserved.

The completion screen is a calm acknowledgment. No confetti, no XP, no streak. `docs/AGENTS.md` explicitly rules out gamification.

Respect reduced motion on the ring animation and the ambient background.
