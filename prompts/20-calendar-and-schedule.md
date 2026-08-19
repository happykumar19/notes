Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 11.1-11.10.

Design reference: `assets/stitch_flowday_executive_productivity_companion/schedule/`

Permissions come first. Request calendar access only at the moment the user enables calendar planning, never at launch, and explain what Flowday does with it before the system dialog appears. Per `docs/AGENTS.md` § Permissions, a denied permission must leave the app fully usable.

Read events with `expo-calendar` and normalize them into a `calendar_events` migration storing only what planning needs: time range, busy state, and a title the user already owns. Do not copy attendee lists, locations, notes, or descriptions into your database or into any AI prompt. Calendar content is among the most sensitive data in the product.

Model AI-generated work blocks in a separate schema from fixed events. These two must never be conflated in the database, in the planner, or in the UI, and the `schedule` design shows them distinctly. Flowday never writes to the user's calendar without an explicit, per-action confirmation.

Build an available-time calculator that finds free windows without touching fixed events, and add transition buffers between context switches, per Task 11.6. Back-to-back scheduling is exactly the failure mode this product exists to prevent.

Feed free windows and fixed events into `PlanningContext` from prompt `14`, and update the planner so no task is ever scheduled over a meeting.

Add `Optimize My Day`, which regenerates the schedule from real availability. Its output is a proposal the user accepts, not a change that has already happened.

`docs/AGENTS.md` § Scheduling Rules again: do not pack every free minute. A three-hour gap does not become three hours of work. Free time is allowed to stay free.
