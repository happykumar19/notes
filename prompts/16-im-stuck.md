Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 8.6-8.10.

Design references:

```txt
assets/stitch_flowday_executive_productivity_companion/i_m_stuck_what_s_wrong/    # reason picker
assets/stitch_flowday_executive_productivity_companion/i_m_stuck_too_much/        # intervention
assets/stitch_flowday_executive_productivity_companion/i_m_stuck_start_small/     # intervention
```

The picker offers exactly the six reasons from Task 8.6: too much to do, don't know where to start, it feels boring, no energy, I'm distracted, I'm avoiding it. Plus the `Just tell me what to do` escape hatch shown in the design, for a user with no capacity to categorize their own state.

Only two of the six interventions were generated. Design the remaining four to match those two exactly in structure and tone.

Each reason maps to a deterministic intervention shape before the AI is involved:

- too much → cut scope to one thing
- don't know where to start → produce a two-minute first step
- boring → shrink it and time-box it
- no energy → swap to a low-energy task or defer without penalty
- distracted → a short reset and a single restart action
- avoiding it → name the smallest non-threatening entry point

The AI fills in the specifics for the current task within that shape. It does not choose the shape.

`docs/AGENTS.md` § I'm Stuck Rules sets the hard constraint: short and actionable, never a motivational speech. Cap the response at roughly two sentences plus one button. A user who is stuck cannot process a paragraph, and a wall of encouragement is a worse outcome than saying nothing.

Every intervention ends in one concrete action the user can take right now.

Entry points, per Task 8.10: Today, task detail, and focus mode. From focus mode the current task and session context are preserved, and the timer keeps running or pauses cleanly rather than being destroyed.

Nothing in this flow implies failure. No "you've been stuck for a while", no streak break, no counter.
