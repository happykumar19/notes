Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 8.1-8.5.

Design references:

```txt
assets/stitch_flowday_executive_productivity_companion/task_detail_auth_refactor/       # the modal, use this one
assets/stitch_flowday_executive_productivity_companion/break_this_down_portfolio_website/
```

`task_detail_client_proposal` is the same screen rendered with a tab bar. Use the modal version, which matches Task 1.10, and treat the other only as a copy reference.

**Task detail** at `/task/[id]`: title, metadata chips for category, estimate, and energy requirement, an AI context note, the breakdown steps, one dominant `Start Focus`, and secondary `Reschedule` and `Tomorrow` actions.

**Task breakdown** at `/task/[id]/breakdown`:

- A decomposition schema of small ordered steps, each with its own estimate.
- A prompt through the reasoning tier of the gateway from prompt `12`, with output validated before persisting to `task_steps`.
- Steps complete individually and persist immediately, with the same optimistic behavior as task completion.
- The primary action is `Start this step`, not `Start task`.

`docs/AGENTS.md` § Task Breakdown Rules governs quality, and it is the whole point of this feature. Steps must be things a person can begin without deciding anything first. "Open the project", "Find the auth route", "Add the first form field" are correct. "Complete authentication implementation" is a failure of the feature, not a stylistic preference.

Do not decompose recursively. Stop when the next action is clear and small. Cap the step count so a task never returns a wall of twenty steps, which recreates the overwhelm the product exists to remove.

Validate that the AI's steps reference a task the user owns, that estimates are bounded and roughly sum to the parent estimate, and that no step is empty.

Fallback when generation fails: keep the task usable with a manual "add a first step" path rather than a dead screen.
