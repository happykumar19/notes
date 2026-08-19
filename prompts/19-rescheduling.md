Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 10.6-10.10.

There is no generated design for this flow. Build it as a bottom sheet using the primitives from prompt `03`, opened from task detail, from a Today task row, and at end of day.

Reschedule reasons, exactly as in Task 10.6: too large, low energy, unexpected work, bad timing, not important, don't know how to start.

Each reason produces a different outcome, which is the entire point of asking:

- too large → offer to break it down instead of just moving it
- don't know how to start → route into the I'm Stuck flow from prompt `16`
- low energy → move to a slot matching the user's known better hours
- bad timing → move to the next realistic window
- unexpected work → move without any friction at all
- not important → offer to drop it from the active plan entirely

Store every reschedule in task history with its reason and increment `reschedule_count`. This is the raw material for the pattern extraction in prompt `22`.

At end of day, identify unfinished active tasks and handle them per `docs/AGENTS.md` § Rescheduling Rules: decide whether each is still important, consider why it stalled, adapt it if possible, move it to a realistic slot, and preserve its context.

Task 10.10 is the hard requirement and the one most likely to be violated by accident: unfinished tasks must never accumulate into an endless overdue list. A task rescheduled repeatedly is a signal that something is wrong with the task, not with the user. Detect that and surface an adapt-or-drop choice instead of moving it a seventh time.

Nothing in this flow shows an overdue badge, a red count, or a days-late label.
