Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 6.8, 6.9, and 6.10.

Design reference: `assets/stitch_flowday_executive_productivity_companion/brain_dump_organized/`

Connect the capture screen from prompt `11` to the extraction gateway from prompt `12`, then let the user approve the result.

Build `/brain-dump/review`:

- Show extracted items grouped as tasks, ideas, reminders, and later items.
- Nothing is written to the database until the user approves. Extraction is a proposal, not a commit.
- Inline editing: rename, delete, merge two items, move an item between groups.
- One dominant action that imports approved items into real tasks.

The processing state is the risk here. Extraction takes seconds, and this is a user who just emptied their head and is waiting. Show something calm and specific, never a bare spinner, and never let it feel stuck.

Failure path: if extraction fails after the gateway's retry and fallback, show the raw dump with a plain retry and an option to save it as a single task. The user's text is preserved in every branch.

On import, create tasks through the existing service from prompt `09` with validation and ownership intact. Mark the brain dump processed. Route back to Today.

Keep the AI's voice inside `docs/AGENTS.md` § AI Should Not Pretend. "It looks like these are tasks" not "I have organized your life."
