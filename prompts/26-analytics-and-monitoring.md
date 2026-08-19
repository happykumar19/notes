Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 17.1-17.7.

**PostHog.** Initialize once, in one place, wrapped in a provider at the root. Identify with the Clerk user id after authentication completes. Set `signup_date` with `$set_once`, and keep onboarding-derived properties current on later identifies.

The event taxonomy is fixed by `docs/AGENTS.md` § Analytics Rules. Use these names exactly:

```txt
brain_dump_created    plan_generated       task_started
task_completed        task_rescheduled     task_abandoned
focus_started         focus_completed      stuck_pressed
rescue_day_used       energy_selected      reflection_completed
notification_opened
```

Properties carry metadata only: durations, counts, energy state, reschedule reason, task size bucket, whether a plan came from AI or the deterministic fallback. Never a task title, never brain dump text, never an AI response, never a calendar event title.

Build one privacy filter that every event passes through, and test that it strips free text. Relying on each call site to remember is how private content leaks into analytics.

Instrument the core loop end to end, per Task 17.3, so the funnel from `docs/todo.md` Task 27.1 is measurable at launch: sign up → energy selected → plan generated → task started.

**Sentry.** Add mobile and server error tracking. Capture crashes, unhandled exceptions, failed API calls, native failures, and AI failures including validation rejections and fallback activations. The AI failure rate is a product metric, not just an error stat.

Attach safe context only: feature, route, operation, environment, and the user id. Scrub request and response bodies, since they contain task content. Then verify it by triggering a real error containing private text and confirming that text does not appear in Sentry.

`docs/AGENTS.md` is explicit that analytics is never an authorization system. Entitlements are checked server-side, never inferred from an event.
