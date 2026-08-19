Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 13.1-13.10.

Design references:

```txt
assets/stitch_flowday_executive_productivity_companion/evening_reflection_2/   # modal, primary
assets/stitch_flowday_executive_productivity_companion/evening_reflection_1/   # wrap-up cards + copy
assets/stitch_flowday_executive_productivity_companion/insights_1/             # tab shell, build first
assets/stitch_flowday_executive_productivity_companion/insights_2/             # content model, build second
```

**Reflection**, at the `/reflection` modal. Use `evening_reflection_2` for the chrome, since Task 1.10 makes reflection a modal, and take the three-card wrap-up and the `Adaptive Learning` copy from `evening_reflection_1`.

The day's wrap-up counts completed, moved, and skipped. Then chips answer "how did the day feel": too much planned, low energy, unexpected work, task was too big, I got distracted, everything went well. Per Task 13.2 this is quick chip selection, never a journal form. A user at the end of a hard day will not type a paragraph, and asking them to is how this feature dies.

Save to a reflection schema, then generate a concise recommendation for tomorrow through the gateway. One short paragraph, ending in a `Set up tomorrow` action.

"2 tasks skipped / Choosing what matters most" is the right framing for the whole screen. Skipped is a decision, not a failure.

**Patterns.** A `user_patterns` migration for derived aggregates: best focus hours, estimation deviation, typical task size, reschedule patterns. Build the extraction service that turns task, focus, and reflection history into those aggregates, and keep derived patterns in their own table, separate from raw activity, per Task 13.7.

**Insights.** Build the `insights_1` layout first: Weekly Rhythm, the energy flow chart, one AI observation, focus wins, and the reflection entry point. Then add the `insights_2` cards as the content deepens: Peak Performance, Planning vs Reality, Less is more.

Task 13.10 is the bar every insight must clear: it leads to a possible behavior change. "You complete tasks 2.1x more often before noon, so schedule your hardest work at 9:30" passes. "You completed 34 tasks" is a number, not an insight, and belongs in a stats card at most.

Insights never rank the user against anyone, never show a downward trend as a warning, and never imply the user is getting worse.
