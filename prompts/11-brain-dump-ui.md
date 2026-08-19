Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 6.1, 6.2, 6.3, and 6.11.

Design reference: `assets/stitch_flowday_executive_productivity_companion/brain_dump_input_1/`

Do not use `brain_dump_input_2`. It belongs to the rejected five-tab family.

Build capture only. AI extraction is prompt `12`.

Create the `brain_dumps` table with a migration, storing raw text, source (text, voice, or paste), and processing status.

Build the `/brain-dump` modal:

- One large free-form text area. Autofocus. Grows with content. Keyboard never covers the input or the primary button.
- `Use voice instead` as a secondary action. Build the interface and the state machine now; the actual transcription is deferred, so it should degrade to a clear "coming soon" rather than a broken button.
- One dominant `Organize this`.
- Draft text survives backgrounding the app and dismissing the modal. Losing a brain dump is the worst possible failure of this screen.

`docs/AGENTS.md` § Brain Dump Rules is strict about this: the user cannot be asked for a project, category, priority, due date, or tag before capturing. No pickers, no required fields, no validation beyond "is there any text".

Add the entry point on Today, matching the `Feeling Stuck? / Start a Brain Dump` card in the `today` design.

Brain dump content is private. It is never sent to analytics and never written to logs.
