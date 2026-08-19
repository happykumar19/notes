Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 7.1-7.15. This is the largest prompt in the sequence and the core product asset. Split it across sessions if needed, but keep the layering intact.

`docs/AGENTS.md` § Planning Engine is the specification. Keep the planning logic isolated in its own module so it can move to `packages/planning/` later without a rewrite. It must be pure and testable without a database or a network.

**Part 1 — Contracts.** `PlanningContext` with energy, available minutes, tasks, calendar events, deadlines, user patterns, and preferences. `PlannedTask`. `DailyPlan` with `theThing`, `wouldBeNice`, `bonus`, `scheduleBlocks`, `nextAction`, and `explanation`. Calendar and pattern fields can be empty for now; Phase 11 and Phase 13 fill them.

**Part 2 — Deterministic rules, written before any AI is involved.** These are the guardrails the model is not allowed to violate:

- Time budget. A plan never exceeds available working time.
- Energy. Task difficulty adapts to the selected state, from Deep Focus down to Overwhelmed.
- Priority. Deadlines, impact, dependencies, and realistic completion probability.
- Task size. Never select an oversized task when the user needs something startable.

`docs/AGENTS.md` is explicit that free time may stay free. The rules must never pack the day just because there is room, and must never fill BONUS to look productive.

**Part 3 — LLM reasoning on top of the rules.** Reasoning tier through the gateway from prompt `12`. Structured output only. Then validate the model's plan against reality: every referenced task exists and belongs to this user, no task is selected twice, the time budget holds, dates are valid, and required fields are present. A plan that fails validation is rejected, not patched.

**Part 4 — Deterministic fallback planner.** When the provider fails or output is unusable, produce a valid plan from the rules alone. The user always gets a plan. This is not optional and it is not a degraded error message.

**Part 5 — Persistence and integration.** The `daily_plans` migration, a service with generate, retrieve, regenerate, and update, and one plan per user per day enforced at the database level so a double tap cannot create two. Then replace the deterministic selection behind the Today contract from prompt `10`. The screen should not need to change. If it does, the contract was wrong.

Add regeneration when energy or available time changes. Regenerating is framed as adjusting, never as correcting a mistake.

Test the rules directly, per `docs/AGENTS.md` § Testing Philosophy: an overloaded day, an empty day, an Overwhelmed user, a day that is entirely meetings, and a single task larger than the whole day.
