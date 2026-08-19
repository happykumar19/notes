
# Phase 0 — Project Definition and Repository Foundation

## 0.1 — Define the initial MVP boundary

* [ ] **Task 0.1 — Freeze the MVP scope**

  * **Depends on:** None
  * Define the first release as:

    * onboarding
    * Clerk authentication
    * Today screen
    * energy check-in
    * tasks
    * Brain Dump
    * AI task triage
    * task breakdown
    * daily planning
    * Focus Mode
    * I'm Stuck
    * Rescue My Day
    * intelligent rescheduling
    * evening reflection
    * basic insights
    * push notifications
    * basic calendar integration
    * subscription foundation
  * Explicitly defer advanced wearable integrations, complex social/body-doubling features, and advanced predictive ML.

* [ ] **Task 0.2 — Define the core product loop**

  * **Depends on:** 0.1
  * Document and preserve this loop:

    ```text
    Capture
      ↓
    Triage
      ↓
    Plan
      ↓
    Choose next action
      ↓
    Start
      ↓
    Focus
      ↓
    Complete / Stuck
      ↓
    Adapt
      ↓
    Reflect
      ↓
    Learn
    ```

* [ ] **Task 0.3 — Define MVP success metrics**

  * **Depends on:** 0.2
  * Define events and metrics around:

    * plan generation
    * task start rate
    * task completion rate
    * focus completion rate
    * Brain Dump conversion
    * Rescue My Day usage
    * rescheduling behavior
    * daily retention
    * weekly retention
  * Focus on whether Flowday helps users **start and complete meaningful work**, not the number of tasks created.

## 0.4 — Establish the repository structure

> **Decision:** Flowday ships as a **single Expo application** at `flowstate/`, managed with npm,
> using Expo API routes for backend endpoints. The Turborepo monorepo described in
> `docs/AGENTS.md` § Architecture remains the eventual target, revisited when the Next.js web app
> begins at Phase 19. Do not restructure the repository as a side effect of a feature.

* [x] **Task 0.4 — Establish the single-app structure**

  * **Depends on:** 0.3
  * The application lives at `flowstate/` with:

    ```text
    flowstate/src/
      app/          # Expo Router routes only
      screens/      # one folder per screen
      components/   # shared UI primitives
      theme/        # design tokens
      constants/
      types/
    supabase/       # migrations, functions, seed (added in Task 2.1)
    ```
  * Module boundaries still matter. Planning logic, AI logic, and validation stay in their own
    modules so they can move to `packages/` later without a rewrite.

* [x] **Task 0.5 — Confirm package management**

  * **Depends on:** 0.4
  * The project uses npm with a committed `package-lock.json`. Do not introduce pnpm or a
    workspace configuration while the repository is a single app.

* [x] **Task 0.6 — Tighten TypeScript configuration**

  * **Depends on:** 0.5
  * Enable strict mode and confirm `typedRoutes` resolves correctly.
  * Establish path aliases so shared types are importable without relative traversal.

* [x] **Task 0.7 — Configure linting and formatting**

  * **Depends on:** 0.6
  * Configure ESLint and a formatter.
  * `expo lint` already exists; extend rather than replace it.

* [x] **Task 0.8 — Add repository quality scripts**

  * **Depends on:** 0.7
  * Add, and ensure each works from `flowstate/`:

    ```bash
    npm run lint
    npm run typecheck
    npm run test
    ```

* [x] **Task 0.9 — Rename the project to Flowday**

  * **Depends on:** 0.8
  * The product is Flowday. Update the `app.json` name, slug, and scheme, the `package.json`
    name, and all user-visible copy currently reading "flowstate".
  * Changing the Expo `scheme` breaks existing deep links and OAuth redirect URIs. Do this before
    Clerk is configured in Task 2.4, not after.
  * The `flowstate/` directory name may be renamed at the same time or left alone; it is not
    user-visible.

---

# Phase 1 — Design System and Mobile Shell

## 1.1 — Create the mobile Expo application

* [ ] **Task 1.1 — Initialize Expo mobile app**

  * **Depends on:** 0.8
  * Create the React Native + Expo application inside `apps/mobile`.

* [x] **Task 1.2 — Configure Expo Router**

  * **Depends on:** 1.1
  * Create route groups:

    ```text
    (auth)
    (tabs)
    ```

* [ ] **Task 1.3 — Configure NativeWind**

  * **Depends on:** 1.2
  * Configure the NativeWind version already selected for the project.
  * Confirm Tailwind styling works on iOS and Android.

* [x] **Task 1.4 — Establish mobile-safe-area and screen conventions**

  * **Depends on:** 1.3
  * Establish consistent:

    * safe-area handling
    * screen containers
    * keyboard handling
    * scrolling
    * loading states
    * error states
  * Implemented via `flowstate/src/components/Screen.tsx` (safe area, scroll,
    keyboard avoidance/dismiss, loading/error slots).

## 1.2 — Implement the Flowday design system

* [x] **Task 1.5 — Define design tokens**

  * **Depends on:** 1.4
  * Define:

    * colors
    * typography
    * spacing
    * border radius
    * shadows
    * icon sizing
    * touch targets
  * Implemented in `flowstate/src/theme/` and mirrored in `flowstate/src/global.css`.

* [x] **Task 1.6 — Implement typography primitives**

  * **Depends on:** 1.5
  * Create reusable typography styles for:

    * screen title
    * section heading
    * task title
    * metadata
    * body
    * AI text
    * button labels
  * Exposed as `typeRoles` / `getTypeRoleStyle` in `flowstate/src/theme/typography.ts`.

* [x] **Task 1.7 — Implement common UI primitives**

  * **Depends on:** 1.6
  * Build:

    * PrimaryButton
    * SecondaryButton
    * Card
    * IconButton
    * Badge
    * Divider
    * EmptyState
    * LoadingState

* [x] **Task 1.8 — Implement Flowday-specific primitives**

  * **Depends on:** 1.7
  * Build:

    * EnergySelector
    * PriorityCard
    * NextActionCard
    * TaskCard
    * AIInsightCard
    * BottomSheet
    * ProgressIndicator

## 1.3 — Implement app navigation shell

* [x] **Task 1.9 — Create bottom tab navigation**

  * **Depends on:** 1.8
  * Implement:

    ```text
    Today
    Tasks
    Focus
    Insights
    ```
  * Four tabs only; AI companion opens from FAB (not a fifth tab).

* [x] **Task 1.10 — Create modal routes**

  * **Depends on:** 1.9
  * Add routes for:

    * Brain Dump
    * Task Detail
    * Task Breakdown
    * Focus
    * I'm Stuck
    * Rescue
    * Schedule
    * Reflection
    * Settings
  * Focus session is `fullScreenModal` with no header; other non-tab routes
    present as modals with back affordance. Companion is also a modal.

* [x] **Task 1.11 — Validate navigation state**

  * **Depends on:** 1.10
  * Ensure app state survives normal navigation transitions.
  * Placeholder screens include a local state probe; tab screens stay mounted.

---

# Phase 2 — Backend and Authentication

## 2.1 — Set up Supabase

* [x] **Task 2.1 — Create Supabase project**

  * **Depends on:** 1.11
  * Create development and production environments.

* [x] **Task 2.2 — Configure PostgreSQL migrations**

  * **Depends on:** 2.1
  * Establish versioned migrations.

* [x] **Task 2.3 — Configure Supabase environment variables**

  * **Depends on:** 2.2
  * Add secure client/server configuration without committing secrets.

## 2.2 — Integrate Clerk

* [ ] **Task 2.4 — Create Clerk application**

  * **Depends on:** 2.3
  * Configure supported authentication providers.
  * App integration is in `flowstate/`. Enable Native API plus email code, Apple, and Google in the Clerk Dashboard. Only the publishable key is stored in `flowstate/.env`.

* [x] **Task 2.5 — Integrate Clerk into Expo**

  * **Depends on:** 2.4
  * Add Clerk provider and session handling to the mobile app.

* [x] **Task 2.6 — Build authentication routes**

  * **Depends on:** 2.5
  * Implement:

    * sign in
    * sign up
    * sign out
    * session restoration

* [ ] **Task 2.7 — Integrate Clerk into Next.js**

  * **Depends on:** 2.6
  * Prepare shared account/session behavior for the web application.

## 2.3 — Establish application user identity

* [x] **Task 2.8 — Create users table**

  * **Depends on:** 2.7
  * Store the Clerk user ID as the external identity.

* [x] **Task 2.9 — Create authenticated user bootstrap flow**

  * **Depends on:** 2.8
  * When a user first authenticates:

    * create or retrieve application profile
    * initialize default preferences
    * initialize onboarding state

* [x] **Task 2.10 — Implement authorization helpers**

  * **Depends on:** 2.9
  * Centralize authenticated user context for backend operations.

* [x] **Task 2.11 — Add Row Level Security**

  * **Depends on:** 2.10
  * Protect user-owned database records.
  * Verify users cannot access another user's data.

---

# Phase 3 — Data Model and Core Task System

## 3.1 — Design database schema

* [x] **Task 3.1 — Create task schema**

  * **Depends on:** 2.11
  * Create:

    ```text
    tasks
    projects
    task_steps
    ```

* [x] **Task 3.2 — Add task lifecycle fields**

  * **Depends on:** 3.1
  * Support:

    * status
    * priority
    * due date
    * estimated duration
    * actual duration
    * completion date
    * reschedule count

* [x] **Task 3.3 — Create user preference schema**

  * **Depends on:** 3.2
  * Store:

    * preferred structure
    * focus duration
    * notification preferences
    * planning preferences

## 3.2 — Build task API

* [ ] **Task 3.4 — Create task repository/service**

  * **Depends on:** 3.3
  * Implement server-side task access.

* [ ] **Task 3.5 — Create task validation schemas**

  * **Depends on:** 3.4
  * Validate:

    * task creation
    * update
    * completion
    * rescheduling

* [ ] **Task 3.6 — Create task CRUD endpoints**

  * **Depends on:** 3.5
  * Implement:

    * create
    * read
    * update
    * delete
    * complete
    * reschedule

## 3.3 — Build task client state

* [ ] **Task 3.7 — Configure TanStack Query**

  * **Depends on:** 3.6
  * Create server-state hooks.

* [ ] **Task 3.8 — Implement task hooks**

  * **Depends on:** 3.7
  * Create:

    * `useTasks`
    * `useTask`
    * `useCreateTask`
    * `useUpdateTask`
    * `useCompleteTask`
    * `useRescheduleTask`

* [ ] **Task 3.9 — Implement optimistic task completion**

  * **Depends on:** 3.8
  * Complete tasks instantly in the UI and synchronize with the server.

* [ ] **Task 3.10 — Build basic task UI**

  * **Depends on:** 3.9
  * Implement task creation, viewing, editing, and completion.

---

# Phase 4 — Onboarding and User Context

## 4.1 — Build onboarding flow

* [ ] **Task 4.1 — Create onboarding state model**

  * **Depends on:** 3.10
  * Store:

    * common blockers
    * focus preferences
    * preferred structure
    * goals
    * energy patterns

* [ ] **Task 4.2 — Build onboarding screens**

  * **Depends on:** 4.1
  * Implement the planned Flowday onboarding sequence.

* [ ] **Task 4.3 — Persist onboarding responses**

  * **Depends on:** 4.2
  * Save onboarding preferences to Supabase.

* [ ] **Task 4.4 — Route users after onboarding**

  * **Depends on:** 4.3
  * New users proceed to the first daily setup experience.
  * Existing users proceed directly to Today.

---

# Phase 5 — Energy System and Today Foundation

## 5.1 — Build energy check-in

* [x] **Task 5.1 — Create energy data model**

  * **Depends on:** 4.4
  * Create:

    ```text
    energy_checkins
    ```
  * Store state and timestamp.

* [x] **Task 5.2 — Create EnergySelector**

  * **Depends on:** 5.1
  * Implement:

    * Deep Focus
    * Normal
    * Scattered
    * Low Energy
    * Overwhelmed

* [x] **Task 5.3 — Save energy check-ins**

  * **Depends on:** 5.2
  * Persist selected state.

* [x] **Task 5.4 — Make current energy available to planning**

  * **Depends on:** 5.3
  * Create a common planning context API.

## 5.2 — Build initial Today screen

* [x] **Task 5.5 — Create Today data contract**

  * **Depends on:** 5.4
  * Define:

    ```text
    theThing
    wouldBeNice[]
    bonus[]
    nextAction
    availableTime
    ```

* [x] **Task 5.6 — Build static Today layout**

  * **Depends on:** 5.5
  * Implement the visual structure before adding AI.

* [x] **Task 5.7 — Connect Today to real tasks**

  * **Depends on:** 5.6
  * Populate the screen from the task system.

* [x] **Task 5.8 — Implement next-action interaction**

  * **Depends on:** 5.7
  * Allow the user to start the current task directly.

---

# Phase 6 — Brain Dump and AI Triage

## 6.1 — Build Brain Dump capture

* [ ] **Task 6.1 — Create brain-dump data model**

  * **Depends on:** 5.8
  * Create:

    ```text
    brain_dumps
    ```

* [ ] **Task 6.2 — Build Brain Dump UI**

  * **Depends on:** 6.1
  * Support large free-form text input.

* [ ] **Task 6.3 — Add voice-capture interface placeholder**

  * **Depends on:** 6.2
  * Add UI architecture for voice capture without blocking the text-first MVP.

## 6.2 — Build AI extraction layer

* [ ] **Task 6.4 — Create AI gateway**

  * **Depends on:** 6.3
  * Provide one internal interface for model calls.

* [ ] **Task 6.5 — Create brain-dump extraction schema**

  * **Depends on:** 6.4
  * Define structured output:

    ```text
    actionableTasks[]
    ideas[]
    reminders[]
    laterItems[]
    ```

* [ ] **Task 6.6 — Create extraction prompt**

  * **Depends on:** 6.5
  * Instruct the model to turn unstructured input into actionable units.

* [ ] **Task 6.7 — Validate AI extraction**

  * **Depends on:** 6.6
  * Add runtime schema validation and fallback handling.

## 6.3 — Connect Brain Dump to tasks

* [ ] **Task 6.8 — Build review screen**

  * **Depends on:** 6.7
  * Show extracted items before saving.

* [ ] **Task 6.9 — Allow edits before import**

  * **Depends on:** 6.8
  * Support:

    * rename
    * delete
    * merge
    * move category

* [ ] **Task 6.10 — Import approved items into tasks**

  * **Depends on:** 6.9
  * Convert approved items into real task records.

* [ ] **Task 6.11 — Add Brain Dump entry point to Today**

  * **Depends on:** 6.10
  * Make Brain Dump accessible from the main daily experience.

---

# Phase 7 — Adaptive Planning Engine

## 7.1 — Define planning context

* [ ] **Task 7.1 — Create PlanningContext types**

  * **Depends on:** 6.11
  * Include:

    * energy
    * available time
    * tasks
    * deadlines
    * calendar events
    * user preferences
    * historical patterns

* [ ] **Task 7.2 — Create PlannedTask schema**

  * **Depends on:** 7.1
  * Define the structured representation of an AI-selected task.

* [ ] **Task 7.3 — Create DailyPlan schema**

  * **Depends on:** 7.2
  * Define:

    ```text
    theThing
    wouldBeNice
    bonus
    scheduleBlocks
    nextAction
    explanation
    ```

## 7.2 — Build rule layer

* [ ] **Task 7.4 — Implement time-budget rules**

  * **Depends on:** 7.3
  * Prevent plans from exceeding available working time.

* [ ] **Task 7.5 — Implement energy rules**

  * **Depends on:** 7.4
  * Adapt task difficulty based on selected energy state.

* [ ] **Task 7.6 — Implement priority rules**

  * **Depends on:** 7.5
  * Prioritize:

    * deadlines
    * impact
    * dependencies
    * realistic completion probability

* [ ] **Task 7.7 — Implement task-size rules**

  * **Depends on:** 7.6
  * Avoid selecting a giant task when the user needs an actionable next step.

## 7.3 — Add LLM reasoning

* [ ] **Task 7.8 — Create daily-planning prompt**

  * **Depends on:** 7.7
  * Tell the model to select a realistic daily workload.

* [ ] **Task 7.9 — Require structured plan output**

  * **Depends on:** 7.8
  * Prevent unstructured planning responses.

* [ ] **Task 7.10 — Add post-AI validation**

  * **Depends on:** 7.9
  * Verify:

    * all tasks exist
    * no duplicate selection
    * time budget is valid
    * dates are valid
    * required fields exist

* [ ] **Task 7.11 — Add deterministic fallback planner**

  * **Depends on:** 7.10
  * Produce a basic valid plan if the AI provider fails.

## 7.4 — Persist daily plans

* [ ] **Task 7.12 — Create daily_plans schema**

  * **Depends on:** 7.11
  * Persist generated daily plans.

* [ ] **Task 7.13 — Build daily plan service**

  * **Depends on:** 7.12
  * Support:

    * generate
    * retrieve
    * regenerate
    * update

* [ ] **Task 7.14 — Connect adaptive plan to Today**

  * **Depends on:** 7.13
  * Replace the initial static Today logic.

* [ ] **Task 7.15 — Add plan regeneration**

  * **Depends on:** 7.14
  * Allow the user to regenerate after changing energy or schedule.

---

# Phase 8 — Task Breakdown and "I'm Stuck"

## 8.1 — Task breakdown

* [ ] **Task 8.1 — Create task-breakdown schema**

  * **Depends on:** 7.15
  * Define small actionable steps.

* [ ] **Task 8.2 — Create decomposition prompt**

  * **Depends on:** 8.1
  * Require concrete first actions rather than broad project descriptions.

* [ ] **Task 8.3 — Build task breakdown service**

  * **Depends on:** 8.2
  * Call the AI gateway and validate output.

* [ ] **Task 8.4 — Build Task Breakdown UI**

  * **Depends on:** 8.3
  * Display sequential steps with estimates.

* [ ] **Task 8.5 — Allow step-by-step completion**

  * **Depends on:** 8.4
  * Persist completion state for each step.

## 8.2 — "I'm Stuck" system

* [ ] **Task 8.6 — Create stuck-reason schema**

  * **Depends on:** 8.5
  * Support:

    * too much
    * don't know where to start
    * boring
    * low energy
    * distracted
    * avoiding it

* [ ] **Task 8.7 — Build stuck reason UI**

  * **Depends on:** 8.6
  * Keep the selection process very short.

* [ ] **Task 8.8 — Build stuck intervention rules**

  * **Depends on:** 8.7
  * Map each reason to an intervention.

* [ ] **Task 8.9 — Build AI stuck response**

  * **Depends on:** 8.8
  * Generate a small, actionable intervention.

* [ ] **Task 8.10 — Add "I'm Stuck" entry points**

  * **Depends on:** 8.9
  * Expose the flow from:

    * Today
    * Task Detail
    * Focus Mode

---

# Phase 9 — Focus Mode

## 9.1 — Build focus session data

* [ ] **Task 9.1 — Create focus_sessions schema**

  * **Depends on:** 8.10
  * Store:

    * task
    * planned duration
    * actual duration
    * start time
    * end time
    * completion state

* [ ] **Task 9.2 — Create focus state store**

  * **Depends on:** 9.1
  * Keep active timer state locally.

## 9.2 — Build Focus Mode

* [ ] **Task 9.3 — Build Focus Mode UI**

  * **Depends on:** 9.2
  * Show:

    * task
    * next action
    * timer
    * pause
    * complete
    * I'm Stuck

* [ ] **Task 9.4 — Implement accurate timer**

  * **Depends on:** 9.3
  * Avoid relying only on frame-counting timers.
  * Calculate elapsed time from timestamps.

* [ ] **Task 9.5 — Implement pause/resume**

  * **Depends on:** 9.4
  * Persist session state safely.

* [ ] **Task 9.6 — Implement focus completion**

  * **Depends on:** 9.5
  * Complete the session and optionally progress the selected task.

* [ ] **Task 9.7 — Connect Focus Mode with task execution**

  * **Depends on:** 9.6
  * Starting a task should be able to launch Focus Mode.

* [ ] **Task 9.8 — Connect Focus Mode to I'm Stuck**

  * **Depends on:** 9.7
  * Preserve the current task and session context.

---

# Phase 10 — Rescue My Day and Intelligent Rescheduling

## 10.1 — Build Rescue My Day

* [ ] **Task 10.1 — Define overload detection rules**

  * **Depends on:** 9.8
  * Detect combinations such as:

    * too many tasks
    * insufficient time
    * excessive overdue work
    * user-selected overwhelmed state

* [ ] **Task 10.2 — Create rescue plan schema**

  * **Depends on:** 10.1
  * Define:

    ```text
    mustHappen[]
    shouldHappen[]
    canWait[]
    explanation
    ```

* [ ] **Task 10.3 — Build rescue planning service**

  * **Depends on:** 10.2
  * Use deterministic rules plus AI reasoning.

* [ ] **Task 10.4 — Build Rescue My Day UI**

  * **Depends on:** 10.3
  * Show what is being kept and deferred.

* [ ] **Task 10.5 — Apply rescue plan**

  * **Depends on:** 10.4
  * Move low-priority items out of today's active plan without deleting them.

## 10.2 — Intelligent rescheduling

* [ ] **Task 10.6 — Define rescheduling reasons**

  * **Depends on:** 10.5
  * Support:

    * too large
    * low energy
    * unexpected work
    * bad timing
    * not important
    * don't know how to start

* [ ] **Task 10.7 — Create rescheduling service**

  * **Depends on:** 10.6
  * Choose a new realistic date/time or task form.

* [ ] **Task 10.8 — Update task history**

  * **Depends on:** 10.7
  * Store rescheduling behavior.

* [ ] **Task 10.9 — Integrate rescheduling into task completion flow**

  * **Depends on:** 10.8
  * When the day ends, identify unfinished active tasks.

* [ ] **Task 10.10 — Ensure unfinished tasks never become an endless overdue pile**

  * **Depends on:** 10.9
  * Preserve context while adapting or deferring tasks.

---

# Phase 11 — Calendar Integration and Scheduling

## 11.1 — Add calendar permissions

* [ ] **Task 11.1 — Create calendar permission flow**

  * **Depends on:** 10.10
  * Request permission only when the user enables calendar planning.

* [ ] **Task 11.2 — Implement calendar event retrieval**

  * **Depends on:** 11.1
  * Retrieve relevant events and normalize them.

## 11.2 — Build schedule model

* [ ] **Task 11.3 — Create calendar_events schema**

  * **Depends on:** 11.2
  * Store normalized calendar information needed for planning.

* [ ] **Task 11.4 — Create schedule block schema**

  * **Depends on:** 11.3
  * Represent AI-generated work blocks separately from fixed events.

* [ ] **Task 11.5 — Create available-time calculator**

  * **Depends on:** 11.4
  * Calculate available windows without overwriting fixed events.

* [ ] **Task 11.6 — Add transition-buffer rules**

  * **Depends on:** 11.5
  * Reserve time between context switches.

## 11.3 — Connect calendar to AI planner

* [ ] **Task 11.7 — Add calendar context to PlanningContext**

  * **Depends on:** 11.6
  * Feed free windows and fixed events into planning.

* [ ] **Task 11.8 — Update planner to respect fixed events**

  * **Depends on:** 11.7
  * Prevent tasks from being scheduled over meetings.

* [ ] **Task 11.9 — Build Schedule screen**

  * **Depends on:** 11.8
  * Distinguish fixed events from AI-generated blocks.

* [ ] **Task 11.10 — Add Optimize My Day action**

  * **Depends on:** 11.9
  * Regenerate the schedule using real calendar availability.

---

# Phase 12 — Notifications and Accountability

## 12.1 — Notification infrastructure

* [ ] **Task 12.1 — Configure Expo notifications**

  * **Depends on:** 11.10
  * Set up iOS and Android notification capabilities.

* [ ] **Task 12.2 — Create notification preferences**

  * **Depends on:** 12.1
  * Allow users to control notification categories.

* [ ] **Task 12.3 — Create notification service**

  * **Depends on:** 12.2
  * Centralize notification scheduling.

## 12.2 — Intelligent notifications

* [ ] **Task 12.4 — Implement morning planning reminder**

  * **Depends on:** 12.3
  * Encourage daily setup without guilt.

* [ ] **Task 12.5 — Implement focus opportunity notification**

  * **Depends on:** 12.4
  * Notify users when an appropriate work window appears.

* [ ] **Task 12.6 — Implement transition notification**

  * **Depends on:** 12.5
  * Prepare users for upcoming planned work.

* [ ] **Task 12.7 — Implement evening reflection reminder**

  * **Depends on:** 12.6
  * Prompt reflection at a suitable time.

* [ ] **Task 12.8 — Add notification throttling**

  * **Depends on:** 12.7
  * Prevent excessive notifications.

* [ ] **Task 12.9 — Add behavioral notification adaptation**

  * **Depends on:** 12.8
  * Start recording which notification timing/actions users respond to.

---

# Phase 13 — Evening Reflection and Insights

## 13.1 — Reflection system

* [ ] **Task 13.1 — Create daily reflection schema**

  * **Depends on:** 12.9
  * Store:

    * what worked
    * blockers
    * skipped reasons
    * plan feedback

* [ ] **Task 13.2 — Build reflection UI**

  * **Depends on:** 13.1
  * Provide quick options rather than a long journal form.

* [ ] **Task 13.3 — Save reflection**

  * **Depends on:** 13.2
  * Persist reflection data.

* [ ] **Task 13.4 — Generate AI reflection summary**

  * **Depends on:** 13.3
  * Produce a concise recommendation for tomorrow.

## 13.2 — User behavior patterns

* [ ] **Task 13.5 — Create user_patterns schema**

  * **Depends on:** 13.4
  * Track derived patterns such as:

    * best focus hours
    * estimation deviation
    * typical task size
    * reschedule patterns

* [ ] **Task 13.6 — Build pattern extraction service**

  * **Depends on:** 13.5
  * Turn historical task/focus/reflection events into aggregate patterns.

* [ ] **Task 13.7 — Store derived patterns safely**

  * **Depends on:** 13.6
  * Keep patterns separate from raw activity data.

## 13.3 — Insights screen

* [ ] **Task 13.8 — Build insights API**

  * **Depends on:** 13.7
  * Return human-readable patterns and recommendations.

* [ ] **Task 13.9 — Build Insights UI**

  * **Depends on:** 13.8
  * Display:

    * strongest work windows
    * estimation patterns
    * task completion patterns
    * planning density

* [ ] **Task 13.10 — Add actionable recommendations**

  * **Depends on:** 13.9
  * Every useful insight should lead to a possible behavior change.

---

# Phase 14 — AI Personal Memory

## 14.1 — Define memory model

* [ ] **Task 14.1 — Define memory categories**

  * **Depends on:** 13.10
  * Separate:

    * preferences
    * stable patterns
    * temporary context
    * planning behavior

* [ ] **Task 14.2 — Define memory retention rules**

  * **Depends on:** 14.1
  * Avoid storing irrelevant or sensitive information indefinitely.

## 14.2 — Implement semantic memory

* [ ] **Task 14.3 — Enable pgvector**

  * **Depends on:** 14.2
  * Add vector support only where semantic retrieval provides actual value.

* [ ] **Task 14.4 — Create memory embedding pipeline**

  * **Depends on:** 14.3
  * Generate embeddings for approved memory records.

* [ ] **Task 14.5 — Build memory retrieval service**

  * **Depends on:** 14.4
  * Retrieve relevant memories for planning contexts.

* [ ] **Task 14.6 — Add memory to PlanningContext**

  * **Depends on:** 14.5
  * Allow daily planning to consider personalized patterns.

* [ ] **Task 14.7 — Add memory correction controls**

  * **Depends on:** 14.6
  * Let users inspect/remove incorrect stored preferences or memories.

---

# Phase 15 — AI Companion

## 15.1 — Build contextual AI actions

* [ ] **Task 15.1 — Create AI action router**

  * **Depends on:** 14.7
  * Support:

    * What should I do now?
    * Rescue my day
    * Break this down
    * Plan tomorrow
    * I'm stuck
    * Brain dump

* [ ] **Task 15.2 — Build contextual AI response schemas**

  * **Depends on:** 15.1
  * Prefer responses containing:

    ```text
    context
    recommendation
    nextAction
    actionButton
    ```

## 15.2 — Build AI companion UI

* [ ] **Task 15.3 — Build AI Companion screen**

  * **Depends on:** 15.2
  * Make it action-oriented rather than a generic chatbot.

* [ ] **Task 15.4 — Add quick actions**

  * **Depends on:** 15.3
  * Make common workflows one tap away.

* [ ] **Task 15.5 — Connect AI Companion to real app actions**

  * **Depends on:** 15.4
  * The AI should be able to trigger validated application operations such as:

    * creating tasks
    * planning
    * rescheduling
    * starting focus

---

# Phase 16 — Widgets and Quick Actions

## 16.1 — Build "Right Now" widget

* [ ] **Task 16.1 — Define widget data contract**

  * **Depends on:** 15.5
  * Show the current next action.

* [ ] **Task 16.2 — Implement iOS/Android widget**

  * **Depends on:** 16.1
  * Display:

    ```text
    RIGHT NOW
    Task
    Estimated time
    START
    ```

* [ ] **Task 16.3 — Connect widget to current plan**

  * **Depends on:** 16.2
  * Keep displayed content synchronized with the Today plan.

## 16.2 — Add additional widget states

* [ ] **Task 16.4 — Build Today widget**

  * **Depends on:** 16.3
  * Display THE THING and secondary tasks.

* [ ] **Task 16.5 — Build Focus widget**

  * **Depends on:** 16.4
  * Display active focus session.

---

# Phase 17 — Analytics, Monitoring, and Product Learning

## 17.1 — Analytics infrastructure

* [ ] **Task 17.1 — Integrate PostHog**

  * **Depends on:** 16.5
  * Add product analytics infrastructure.

* [ ] **Task 17.2 — Create event taxonomy**

  * **Depends on:** 17.1
  * Standardize event names and properties.

* [ ] **Task 17.3 — Instrument core product loop**

  * **Depends on:** 17.2
  * Track:

    * plan generated
    * task started
    * task completed
    * focus started
    * focus completed
    * stuck
    * rescue
    * reflection

* [ ] **Task 17.4 — Add privacy filtering**

  * **Depends on:** 17.3
  * Ensure raw private content is not sent to analytics.

## 17.2 — Error monitoring

* [ ] **Task 17.5 — Integrate Sentry**

  * **Depends on:** 17.4
  * Add mobile and server error tracking.

* [ ] **Task 17.6 — Add contextual error metadata**

  * **Depends on:** 17.5
  * Include safe identifiers:

    * feature
    * route
    * operation
    * environment

* [ ] **Task 17.7 — Verify sensitive data is excluded**

  * **Depends on:** 17.6
  * Confirm tokens, private content, and secrets are never reported.

---

# Phase 18 — Monetization

## 18.1 — Define entitlements

* [ ] **Task 18.1 — Define Free tier**

  * **Depends on:** 17.7
  * Decide exactly what remains free.

* [ ] **Task 18.2 — Define Pro tier**

  * **Depends on:** 18.1
  * Potentially include:

    * unlimited AI
    * adaptive scheduling
    * advanced insights
    * memory
    * voice
    * advanced calendar planning

## 18.2 — Integrate RevenueCat

* [ ] **Task 18.3 — Configure RevenueCat project**

  * **Depends on:** 18.2
  * Configure products and entitlements.

* [ ] **Task 18.4 — Implement subscription state**

  * **Depends on:** 18.3
  * Retrieve entitlement state in the app.

* [ ] **Task 18.5 — Build paywall**

  * **Depends on:** 18.4
  * Explain product value clearly without dark patterns.

* [ ] **Task 18.6 — Protect premium backend features**

  * **Depends on:** 18.5
  * Verify premium access server-side where necessary.

* [ ] **Task 18.7 — Implement restore purchases**

  * **Depends on:** 18.6
  * Support App Store/Google Play restoration.

---

# Phase 19 — Web Application

## 19.1 — Build Next.js foundation

* [ ] **Task 19.1 — Initialize Next.js web app**

  * **Depends on:** 18.7
  * Add the web application to the monorepo.

* [ ] **Task 19.2 — Integrate Clerk**

  * **Depends on:** 19.1
  * Use the same authentication identity as mobile.

* [ ] **Task 19.3 — Connect shared types**

  * **Depends on:** 19.2
  * Reuse common product types and validation.

## 19.2 — Marketing site

* [ ] **Task 19.4 — Build landing page**

  * **Depends on:** 19.3
  * Explain:

    > "Flowday helps you figure out what to do next — and actually start."

* [ ] **Task 19.5 — Build feature sections**

  * **Depends on:** 19.4
  * Showcase:

    * Adaptive Day
    * Brain Dump
    * I'm Stuck
    * Rescue My Day
    * Focus Mode
    * AI personalization

* [ ] **Task 19.6 — Build pricing page**

  * **Depends on:** 19.5
  * Connect the pricing presentation to the actual entitlements.

## 19.3 — Web product foundation

* [ ] **Task 19.7 — Build authenticated web shell**

  * **Depends on:** 19.6
  * Add navigation and account state.

* [ ] **Task 19.8 — Build web Today view**

  * **Depends on:** 19.7
  * Reuse the same daily-plan concepts.

---

# Phase 20 — Admin and Operations

## 20.1 — Admin authentication

* [ ] **Task 20.1 — Create admin role model**

  * **Depends on:** 19.8
  * Separate admin privileges from normal users.

* [ ] **Task 20.2 — Protect admin routes**

  * **Depends on:** 20.1
  * Enforce authorization server-side.

## 20.2 — Admin features

* [ ] **Task 20.3 — Build user support view**

  * **Depends on:** 20.2
  * Show safe operational information without exposing unnecessary private content.

* [ ] **Task 20.4 — Build product metrics dashboard**

  * **Depends on:** 20.3
  * Show:

    * active users
    * plan generation
    * task starts
    * completions
    * retention
    * subscription metrics

* [ ] **Task 20.5 — Build AI health monitoring**

  * **Depends on:** 20.4
  * Monitor:

    * AI failure rate
    * fallback rate
    * latency
    * token/cost estimates

---

# Phase 21 — Testing and Hardening

## 21.1 — Unit testing

* [ ] **Task 21.1 — Test task business logic**

  * **Depends on:** 20.5
  * Test:

    * completion
    * rescheduling
    * priorities
    * time calculations

* [ ] **Task 21.2 — Test planning rules**

  * **Depends on:** 21.1
  * Test:

    * time budget
    * energy adaptation
    * priority logic
    * task sizing

* [ ] **Task 21.3 — Test rescue rules**

  * **Depends on:** 21.2
  * Verify overloaded days produce realistic plans.

* [ ] **Task 21.4 — Test AI schema validation**

  * **Depends on:** 21.3
  * Test malformed and incomplete model outputs.

## 21.2 — Integration testing

* [ ] **Task 21.5 — Test Clerk authentication**

  * **Depends on:** 21.4
  * Test:

    * sign in
    * sign out
    * account creation
    * session restoration

* [ ] **Task 21.6 — Test database authorization**

  * **Depends on:** 21.5
  * Verify users cannot read or mutate another user's data.

* [ ] **Task 21.7 — Test task API**

  * **Depends on:** 21.6
  * Test the complete task lifecycle.

* [ ] **Task 21.8 — Test planning API**

  * **Depends on:** 21.7
  * Verify real user/task context produces valid plans.

## 21.3 — End-to-end testing

* [ ] **Task 21.9 — Test onboarding**

  * **Depends on:** 21.8
  * New user → onboarding → first plan.

* [ ] **Task 21.10 — Test daily execution flow**

  * **Depends on:** 21.9
  * Today → Start → Focus → Complete.

* [ ] **Task 21.11 — Test stuck flow**

  * **Depends on:** 21.10
  * Today → I'm Stuck → intervention → resume.

* [ ] **Task 21.12 — Test rescue flow**

  * **Depends on:** 21.11
  * Overloaded day → rescue → updated plan.

* [ ] **Task 21.13 — Test end-of-day flow**

  * **Depends on:** 21.12
  * Finish day → reflection → updated patterns.

---

# Phase 22 — Security, Privacy, and Production Readiness

## 22.1 — Security review

* [ ] **Task 22.1 — Audit environment variables**

  * **Depends on:** 21.13
  * Ensure no secrets are committed.

* [ ] **Task 22.2 — Audit Clerk integration**

  * **Depends on:** 22.1
  * Verify client/server boundaries.

* [ ] **Task 22.3 — Audit Supabase RLS**

  * **Depends on:** 22.2
  * Test all user-owned tables.

* [ ] **Task 22.4 — Audit server-side authorization**

  * **Depends on:** 22.3
  * Confirm every protected mutation checks ownership.

* [ ] **Task 22.5 — Audit AI endpoints**

  * **Depends on:** 22.4
  * Confirm provider credentials are never exposed to mobile clients.

## 22.2 — Privacy review

* [ ] **Task 22.6 — Audit analytics payloads**

  * **Depends on:** 22.5
  * Remove private text and sensitive content from events.

* [ ] **Task 22.7 — Audit logging**

  * **Depends on:** 22.6
  * Remove private task/calendar/AI content from logs.

* [ ] **Task 22.8 — Review data retention**

  * **Depends on:** 22.7
  * Define retention/deletion rules.

* [ ] **Task 22.9 — Implement account/data deletion flow**

  * **Depends on:** 22.8
  * Ensure user deletion removes associated application data appropriately.

---

# Phase 23 — Performance and Mobile Polish

## 23.1 — Performance

* [ ] **Task 23.1 — Measure startup performance**

  * **Depends on:** 22.9
  * Measure cold-start and initial navigation.

* [ ] **Task 23.2 — Optimize Today rendering**

  * **Depends on:** 23.1
  * Remove unnecessary renders and data requests.

* [ ] **Task 23.3 — Optimize task lists**

  * **Depends on:** 23.2
  * Ensure long lists stay responsive.

* [ ] **Task 23.4 — Optimize image/assets**

  * **Depends on:** 23.3
  * Remove unnecessary large assets.

## 23.2 — UX polish

* [ ] **Task 23.5 — Polish loading states**

  * **Depends on:** 23.4
  * Make asynchronous operations feel intentional.

* [ ] **Task 23.6 — Polish error states**

  * **Depends on:** 23.5
  * Provide recovery actions.

* [ ] **Task 23.7 — Polish empty states**

  * **Depends on:** 23.6
  * Encourage the correct next action.

* [ ] **Task 23.8 — Add subtle animations**

  * **Depends on:** 23.7
  * Add motion for:

    * task completion
    * plan changes
    * focus transitions
    * bottom sheets
  * Keep motion restrained.

* [ ] **Task 23.9 — Accessibility pass**

  * **Depends on:** 23.8
  * Verify:

    * font scaling
    * screen readers
    * contrast
    * touch targets
    * reduced motion

---

# Phase 24 — App Store and Play Store Preparation

## 24.1 — Production builds

* [ ] **Task 24.1 — Configure EAS development profile**

  * **Depends on:** 23.9
  * Establish repeatable development builds.

* [ ] **Task 24.2 — Configure EAS preview profile**

  * **Depends on:** 24.1
  * Create QA distribution build.

* [ ] **Task 24.3 — Configure production profile**

  * **Depends on:** 24.2
  * Prepare release builds.

## 24.2 — Store assets

* [ ] **Task 24.4 — Create app icon**

  * **Depends on:** 24.3
  * Use the approved Flowday logo and platform-specific requirements.

* [ ] **Task 24.5 — Create splash screen**

  * **Depends on:** 24.4
  * Keep it minimal and consistent with branding.

* [ ] **Task 24.6 — Create App Store screenshots**

  * **Depends on:** 24.5
  * Showcase:

    * Today
    * Brain Dump
    * Focus
    * I'm Stuck
    * Rescue My Day

* [ ] **Task 24.7 — Create Google Play screenshots**

  * **Depends on:** 24.6
  * Adapt the same product story to Android.

* [ ] **Task 24.8 — Write store listing**

  * **Depends on:** 24.7
  * Position Flowday around:

    * adaptive planning
    * task initiation
    * focus
    * reduced overwhelm

---

# Phase 25 — Beta Release

## 25.1 — Internal QA

* [ ] **Task 25.1 — Run full internal regression**

  * **Depends on:** 24.8
  * Test every core user journey.

* [ ] **Task 25.2 — Test multiple device sizes**

  * **Depends on:** 25.1
  * Test representative iOS and Android devices.

* [ ] **Task 25.3 — Test slow/offline connectivity**

  * **Depends on:** 25.2
  * Verify graceful degradation.

* [ ] **Task 25.4 — Test notification behavior**

  * **Depends on:** 25.3
  * Test permissions, timing, deep links, and duplicates.

## 25.2 — Beta users

* [ ] **Task 25.5 — Launch closed beta**

  * **Depends on:** 25.4
  * Distribute through TestFlight and Google Play testing.

* [ ] **Task 25.6 — Collect qualitative feedback**

  * **Depends on:** 25.5
  * Ask:

    * What helped you start?
    * What felt overwhelming?
    * Was the daily plan realistic?
    * Did "I'm Stuck" help?
    * Which features felt unnecessary?

* [ ] **Task 25.7 — Analyze behavioral data**

  * **Depends on:** 25.6
  * Compare:

    * plan creation
    * task starts
    * task completion
    * focus sessions
    * rescheduling
    * retention

* [ ] **Task 25.8 — Prioritize beta fixes**

  * **Depends on:** 25.7
  * Fix problems affecting the core execution loop before adding new features.

---

# Phase 26 — Launch

## 26.1 — Release candidate

* [ ] **Task 26.1 — Freeze core feature set**

  * **Depends on:** 25.8
  * Stop adding major features.

* [ ] **Task 26.2 — Resolve release-blocking bugs**

  * **Depends on:** 26.1
  * Fix crashes, broken auth, data loss, broken planning, and subscription issues.

* [ ] **Task 26.3 — Final production database migration**

  * **Depends on:** 26.2
  * Verify migrations are reproducible.

* [ ] **Task 26.4 — Final production environment audit**

  * **Depends on:** 26.3
  * Verify all production credentials and services.

## 26.2 — Publish

* [ ] **Task 26.5 — Submit iOS build**

  * **Depends on:** 26.4

* [ ] **Task 26.6 — Submit Android build**

  * **Depends on:** 26.5

* [ ] **Task 26.7 — Monitor launch**

  * **Depends on:** 26.6
  * Monitor:

    * crashes
    * auth failures
    * AI failures
    * database errors
    * notification failures
    * subscriptions

---

# Phase 27 — Post-Launch Product Learning

## 27.1 — Analyze the core loop

* [ ] **Task 27.1 — Measure first-session activation**

  * **Depends on:** 26.7
  * Track whether new users:

    ```text
    sign up
      ↓
    choose energy
      ↓
    generate plan
      ↓
    start task
    ```

* [ ] **Task 27.2 — Measure task-start rate**

  * **Depends on:** 27.1
  * Determine whether Flowday actually helps people begin.

* [ ] **Task 27.3 — Measure task-completion rate**

  * **Depends on:** 27.2
  * Compare completion against plan size and task size.

* [ ] **Task 27.4 — Measure rescue effectiveness**

  * **Depends on:** 27.3
  * Determine whether Rescue My Day improves execution instead of simply moving tasks.

## 27.2 — Improve personalization

* [ ] **Task 27.5 — Identify common user failure patterns**

  * **Depends on:** 27.4
  * Find:

    * overplanning
    * underestimation
    * task avoidance
    * notification fatigue
    * oversized tasks

* [ ] **Task 27.6 — Improve planning heuristics**

  * **Depends on:** 27.5
  * Adjust deterministic rules based on evidence.

* [ ] **Task 27.7 — Improve AI planning prompts**

  * **Depends on:** 27.6
  * Improve output quality without relying only on larger models.

* [ ] **Task 27.8 — Evaluate new personalization features**

  * **Depends on:** 27.7
  * Only add new complexity where measurable user benefit exists.

---

# Dependency Chain — Major Milestones

The entire project follows this dependency progression:

```text
0  Product scope
 ↓
1  Repository + mobile shell
 ↓
2  Clerk + Supabase
 ↓
3  Task system
 ↓
4  Onboarding
 ↓
5  Energy + Today
 ↓
6  Brain Dump
 ↓
7  Adaptive Planning Engine
 ↓
8  Task Breakdown + I'm Stuck
 ↓
9  Focus Mode
 ↓
10 Rescue + Rescheduling
 ↓
11 Calendar
 ↓
12 Notifications
 ↓
13 Reflection + Insights
 ↓
14 Personal Memory
 ↓
15 AI Companion
 ↓
16 Widgets
 ↓
17 Analytics + Sentry
 ↓
18 RevenueCat
 ↓
19 Web
 ↓
20 Admin
 ↓
21 Testing
 ↓
22 Security + Privacy
 ↓
23 Performance + Polish
 ↓
24 Store Preparation
 ↓
25 Beta
 ↓
26 Launch
 ↓
27 Product Learning
```

There are **no intentional orphan features** in this plan: each later capability consumes or extends something built earlier.

---

# Recommended MVP Cut Line

Although the full roadmap above is comprehensive, I would **not build all 27 phases before releasing something**.

The first usable Flowday should stop around **Phase 12**, with the following loop working end-to-end:

```text
Sign up
   ↓
Onboarding
   ↓
Energy check-in
   ↓
Brain Dump
   ↓
AI extracts tasks
   ↓
AI creates daily plan
   ↓
THE THING
   ↓
Break task down
   ↓
START
   ↓
Focus Mode
   ↓
Complete / I'm Stuck
   ↓
Reschedule if needed
   ↓
Evening reflection
```

