Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 12.1-12.9.

Design reference for the preferences screen: `assets/stitch_flowday_executive_productivity_companion/notifications_nudges/`

Configure `expo-notifications` for iOS and Android, requesting permission only when the user enables reminders, and build a preferences screen where each category can be turned off independently.

Centralize all scheduling in one notification service. No screen schedules a notification directly, or the throttling in Task 12.8 becomes impossible to enforce.

The four notification types:

- Morning planning reminder, encouraging daily setup
- Focus opportunity, when a suitable work window appears
- Transition, preparing the user for planned work
- Evening reflection, at a suitable time

`docs/AGENTS.md` § Notifications defines the voice, and it is the whole feature. "Your focus window starts in 10 minutes. Want to start the proposal?" is correct. "You still haven't completed your task" is banned, along with every variant: no unfinished counts, no streak warnings, no time-wasted framing, no guilt of any kind.

Throttling is a correctness requirement, not polish. Cap per-day volume, enforce a minimum gap, respect quiet hours, and never fire during an active focus session. For this user, notification fatigue means uninstalling the app.

Every notification deep-links to the exact screen it refers to. A tap that lands on a generic Today screen wastes the interruption.

Start recording which timings and types are acted on, per Task 12.9. Record the metadata only. Notification content is never written to analytics.

Prefer fewer intelligent notifications over more reminders. If a notification would not change what the user does in the next ten minutes, do not send it.
