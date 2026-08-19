Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 10.1-10.5.

Design reference: `assets/stitch_flowday_executive_productivity_companion/rescue_my_day/`

Overload detection, running against the current plan: too many tasks for the available time, excessive overdue work, or the user selecting the Overwhelmed energy state. When detected, surface the rescue entry point on Today. Suggest it, never force it, and never announce that the user is failing.

The rescue plan schema is `mustHappen[]`, `shouldHappen[]`, `canWait[]`, plus an explanation. The design labels these MUST HAPPEN, SHOULD HAPPEN, and CAN WAIT. Keep them in sync.

Build it with deterministic rules first and AI reasoning second, the same layering as prompt `14`. The rules decide what can be cut; the model explains the reasoning in the user's own context.

`docs/AGENTS.md` § Rescue My Day Rules is the specification: stop adding work, identify what truly matters, remove low-value tasks from today's active plan, preserve them for later, and end with one clear next action.

The single most important implementation detail: deferred tasks are moved out of the active plan, never deleted, never marked failed, and never sent to an overdue pile. The design's footer, "Nothing was deleted. Everything can be revisited later when you have more bandwidth," is a promise the code must actually keep. Verify it by checking that every deferred task is still retrievable and still carries its context.

The primary action is `Start with the proposal`, matching `docs/AGENTS.md` § Primary UX Rule. Rescue ends with the user starting something, not with the user admiring a shorter list. A secondary `Review changes` shows exactly what moved before applying.

Copy is blameless throughout. "You have 11 open tasks, but only about 3 hours available" is a fact. "You're falling behind" is banned.
