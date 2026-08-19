# AGENTS.md

You are an expert React Native + Expo engineer helping build **Flowday**, a production-quality AI-powered executive-function and daily execution companion for iOS and Android.

You should think like a senior mobile/product engineer, but implement with simple, maintainable patterns. Prefer clarity, consistency, and reliable user experience over unnecessary abstraction.

This file is the source of truth for development decisions in this repository.

---

## Project Overview

We are building **Flowday**, an AI-powered adaptive productivity app for people who struggle with:

- task initiation
- overwhelm
- procrastination
- time estimation
- inconsistent energy
- distraction
- planning paralysis
- executive-function overload

The product is inspired by ADHD-friendly productivity principles, but it should not feel clinical or medical.

The core product philosophy is:

> **Don't help the user organize more. Help them actually do the thing.**

Flowday should continuously help the user answer:

> **"What should I do right now?"**

The app should:

- capture brain dumps
- turn large goals into small actions
- create realistic daily plans
- adapt plans to energy and available time
- prioritize one important task
- provide focus sessions
- provide an "I'm Stuck" rescue flow
- rescue overloaded days
- intelligently reschedule unfinished tasks
- learn the user's execution patterns
- provide reflective insights
- integrate with calendars and notifications
- use AI as an execution companion rather than a generic chatbot

---

## Product Principles

### 1. Execution over organization

The app must optimize for action, not for maintaining a perfect task database.

A screen showing 30 tasks is usually worse than a screen showing the one correct next action.

### 2. One obvious next action

Every important experience should answer:

> What do I do next?

### 3. Reduce cognitive load

Avoid forcing users to make unnecessary decisions.

Prefer:

- sensible defaults
- progressive disclosure
- AI suggestions
- simple choices
- small next steps

### 4. No guilt-based UX

Never use language such as:

- You failed
- You're behind
- You should have done this
- You missed your goal

Instead use:

- Let's adjust the plan
- We can move this
- This may be too large; let's break it down
- You still made progress

### 5. Flexible plans

Plans are suggestions, not contracts.

The system should adapt when:

- energy changes
- meetings move
- tasks take longer
- new urgent work appears
- the user gets distracted
- the user becomes overwhelmed

### 6. Calm by default

The product should feel supportive even when the user is overloaded.

Do not create a visually stressful productivity dashboard.

---

## Target Users

Primary users:

- people with ADHD or executive-function difficulties
- neurodivergent users
- people who struggle with procrastination and overwhelm

Secondary users:

- developers
- founders
- students
- creators
- knowledge workers
- anyone who wants adaptive productivity

The UI should work for both ADHD users and broader users without medicalizing the experience.

---

## Core User Flows

The first version should revolve around these experiences:

1. Onboarding
2. Daily energy check-in
3. Today / adaptive daily plan
4. Brain dump
5. AI task triage
6. Task breakdown
7. Task detail
8. Start next action
9. Focus mode
10. I'm Stuck
11. Rescue My Day
12. Calendar-aware planning
13. Intelligent rescheduling
14. Evening reflection
15. Personal insights
16. AI companion
17. Notifications and reminders
18. Subscription / account settings

---

## Core Concepts

### Energy States

Use these user-facing states:

- Deep Focus
- Normal
- Scattered
- Low Energy
- Overwhelmed

The selected state can influence:

- task selection
- task size
- estimated effort
- schedule density
- notification style
- AI recommendations

Do not treat these states as medical diagnoses.

### Daily Priority Model

Every daily plan should prioritize:

#### THE THING

The single most important task.

#### WOULD BE NICE

Secondary tasks that fit realistically.

#### BONUS

Optional tasks only if the day goes unusually well.

Do not automatically fill the day with tasks just because there is free time.

### Special Actions

The product should support:

- Start
- Break This Down
- I'm Stuck
- Rescue My Day
- Brain Dump
- Reschedule
- Make It Easier
- I Have Less Time

---

## Tech Stack

Use the following stack unless there is a strong technical reason to change it.

### Mobile

- Expo
- React Native
- TypeScript
- Expo Router
- NativeWind / Tailwind CSS
- TanStack Query
- Zustand

### Authentication

- Clerk

Do not build custom authentication.

Clerk is the authentication source of truth.

### Backend and Database

- Supabase
- PostgreSQL
- Supabase Storage
- Supabase Realtime
- Supabase Edge Functions where appropriate
- pgvector for personal memory when needed

### Web

- Next.js
- TypeScript
- Tailwind CSS

Use the web app for:

- marketing site
- account management
- pricing
- web productivity experience when implemented
- admin tools

### Product Analytics

- PostHog

### Error Monitoring

- Sentry

### Subscriptions

- RevenueCat

### Deployment

- Expo EAS for mobile builds
- Vercel for Next.js
- Supabase for database/backend infrastructure

### Package Management

- pnpm

### Monorepo

- Turborepo

---

## Technology Constraints

Do not introduce new major libraries unless there is a clear benefit.

Before adding a library:

1. Check whether Expo, React Native, Next.js, Supabase, or an existing internal utility already solves the problem.
2. If a new dependency is still useful, explain why.
3. Keep the dependency surface small.
4. Prefer well-supported libraries.

Do not replace the existing stack without a strong reason.

---

## Architecture

Prefer a monorepo with a shared package structure.

Suggested structure:

```txt
apps/
  mobile/
  web/
  admin/

packages/
  ui/
  types/
  validation/
  planning/
  ai/
  database/
  config/
  utils/

supabase/
  migrations/
  functions/
  seed/

assets/
```

The exact structure can evolve, but keep boundaries clear.

---

## Mobile App Structure

Suggested Expo structure:

```txt
apps/mobile/
  app/
    (auth)/
    (tabs)/
      today/
      tasks/
      focus/
      insights/
    brain-dump/
    task/
    focus/
    stuck/
    rescue/
    schedule/
    reflection/
    settings/

  components/
  features/
    today/
    tasks/
    brain-dump/
    focus/
    planning/
    insights/
    calendar/
    notifications/

  hooks/
  lib/
  store/
  types/
  constants/
  assets/
```

Use feature-oriented organization when it improves discoverability.

---

## app/ Rules

Route files should primarily:

- compose components
- call hooks
- connect state
- handle navigation
- pass data into feature components

Do not place:

- large reusable UI blocks
- complex AI logic
- database queries
- large business rules

directly inside route files.

---

## components/ Rules

Create reusable components when:

- a UI pattern appears more than once
- it represents a clear product concept
- it makes a screen significantly easier to understand
- it is likely to be reused

Good examples:

```txt
TaskCard
PriorityCard
EnergySelector
NextActionCard
AIInsightCard
FocusTimer
StuckOptionCard
DailyPlanSection
PrimaryButton
BottomSheet
```

Do not create tiny wrappers just to increase abstraction.

Prefer meaningful components over component proliferation.

---

## Feature Architecture

When a feature becomes non-trivial, organize it as:

```txt
features/
  feature-name/
    components/
    hooks/
    services/
    types.ts
    utils.ts
```

Keep feature-specific logic close to the feature.

Shared logic belongs in shared packages.

---

## Backend Architecture

Do not let every screen directly call an LLM.

Use a consistent flow:

```txt
Mobile
  ↓
API / Server Function
  ↓
Context Builder
  ↓
Planning Engine
  ↓
AI Provider
  ↓
Structured Output Validation
  ↓
Business Rules / Safety Checks
  ↓
Persist Result
  ↓
Mobile
```

The AI should be a reasoning component, not the system of record.

---

## Planning Engine

The adaptive planning engine is a core product asset.

Keep the planning logic isolated in:

```txt
packages/planning/
```

Recommended capabilities:

```ts
generateDailyPlan()
breakDownTask()
calculateNextAction()
rescueDay()
adaptSchedule()
rescheduleTask()
generateReflection()
generateInsights()
```

The planning engine should accept structured context such as:

```ts
type PlanningContext = {
  energy: EnergyState;
  availableMinutes: number;
  tasks: Task[];
  calendarEvents: CalendarEvent[];
  deadlines: Deadline[];
  userPatterns: UserPatterns;
  preferences: UserPreferences;
};
```

Return structured results.

Never rely on parsing free-form AI prose for critical application logic.

---

## AI Output Rules

Use schemas and runtime validation.

Preferred pattern:

```txt
LLM
 ↓
Structured JSON
 ↓
Schema validation
 ↓
Application rules
 ↓
Persist
```

Never assume the model returned valid data.

Validate:

- required fields
- enum values
- dates
- times
- IDs
- task references
- duration limits
- array sizes

If invalid:

- retry when appropriate
- use a fallback
- surface a safe error
- never crash the application

---

## AI Provider Rules

Do not tightly couple the product to one model provider.

Use an internal AI gateway.

For example:

```txt
AI Gateway
  ├── fast model
  ├── reasoning model
  ├── voice model
  └── fallback provider
```

Use the appropriate model for the task.

Examples:

- brain-dump extraction → fast/cheap model
- task classification → fast model
- daily planning → stronger reasoning model
- project decomposition → reasoning model
- stuck conversation → fast conversational model
- transcription → dedicated speech model

Never put provider-specific code throughout the mobile UI.

---

## AI Prompt Rules

Prompts should be:

- versioned
- testable
- concise
- explicit about output schema
- isolated from UI code

Store prompts in the AI/planning layer rather than inside screens.

When a prompt changes, consider whether the behavior requires a version update.

---

## Personal Memory

Personalization is a core differentiator.

Store useful behavioral patterns such as:

- preferred focus windows
- task estimation bias
- preferred task size
- common rescheduling patterns
- typical energy patterns
- planning density preferences
- successful working contexts

Do not store unnecessary sensitive information.

Use pgvector only when semantic memory actually provides value.

Do not introduce embeddings everywhere just because they are available.

---

## User Data and Privacy

Flowday handles potentially sensitive productivity information.

Follow privacy-by-design principles.

Do not log:

- raw personal notes unnecessarily
- full AI conversations in analytics
- private calendar details into third-party analytics
- authentication secrets
- API keys
- access tokens

Use the minimum data required.

When logging events, prefer metadata such as:

```txt
task_started
task_completed
focus_started
rescue_day_used
```

rather than raw private content.

---

## Clerk Rules

Use Clerk for authentication.

Support, as appropriate:

- email
- password
- Apple
- Google
- passkeys / OTP when supported by the product configuration

Never:

- implement password storage
- build a custom auth system
- store authentication secrets in the database
- expose Clerk secret keys in the client

Use the Clerk user ID as the external identity identifier.

---

## Database Identity Rules

Keep authentication identity and application profile separate.

Example:

```txt
Clerk User
    ↓
users.clerk_user_id
    ↓
internal user profile
    ↓
preferences
    ↓
behavior patterns
    ↓
planning memory
```

Database rows should contain the Clerk user ID or internal user ID consistently.

Do not mix random identity formats across tables.

---

## Supabase Security

Use Row Level Security for user-owned data.

Every user-specific table should have explicit ownership rules.

Never trust a client-supplied user ID for authorization.

Derive the authenticated identity from the verified Clerk/session context.

Backend functions must validate:

- authenticated user
- resource ownership
- authorization
- input schema

before reading or mutating user data.

---

## Database Rules

Use PostgreSQL as the source of truth for server-persisted data.

Keep migrations versioned.

Do not manually edit production schemas without a migration.

Use clear table names.

Prefer:

```txt
tasks
task_steps
daily_plans
focus_sessions
calendar_events
energy_checkins
user_preferences
user_patterns
```

Avoid:

```txt
stuff
data
misc
temp
new_table
```

---

## Local State

Use Zustand for client-side global state.

Good examples:

- temporary planning UI state
- active focus session
- selected energy state
- navigation-related state
- local preferences

Do not use Zustand as a replacement for server state.

Use TanStack Query for:

- tasks from the backend
- plans
- insights
- calendar data
- AI results
- server mutations

---

## Offline / Optimistic UX

Important user actions should feel instant.

For example:

```txt
User completes task
  ↓
UI updates immediately
  ↓
Local state persists
  ↓
Server sync happens
```

Do not force users to wait for a network request before seeing a basic task action complete.

Handle:

- offline
- retry
- optimistic updates
- sync conflicts

gracefully.

---

## AsyncStorage

Use AsyncStorage for small local persistence where appropriate.

Do not store:

- secrets
- API keys
- authentication tokens manually
- large datasets

Use appropriate secure storage/auth mechanisms for sensitive data.

---

## Styling Rules

Use NativeWind / Tailwind CSS for styling whenever possible.

Use the NativeWind version already installed in the project.

Before changing styling configuration:

1. Check `package.json`.
2. Check current NativeWind setup.
3. Follow the syntax supported by that version.
4. Do not upgrade NativeWind without approval.

Do not import examples from an incompatible NativeWind version.

---

## Style Exceptions

Use `StyleSheet` or inline styles when required for:

- native-only props
- dynamic calculated styles
- animations
- platform-specific styles
- shadow differences
- transforms
- `contentContainerStyle`
- native component behavior not supported through `className`

Do not force Tailwind classes onto APIs that do not support them correctly.

---

## UI Quality Bar

The product should feel:

- calm
- intelligent
- premium
- warm
- supportive
- modern
- minimal
- highly usable
- mobile-first

It should NOT feel:

- clinical
- childish
- corporate
- like a project-management dashboard
- like a medical application
- excessively gamified

---

## Visual Language

Recommended visual direction:

- warm off-white / cream background
- charcoal typography
- muted sage / mint primary accent
- subtle amber for attention
- muted coral for overload/urgent states
- restrained blue for informational states

Avoid:

- purple gradients
- neon gradients
- excessive glassmorphism
- giant shadows
- excessive borders
- excessive color
- emoji-heavy interfaces

---

## Typography

Use a highly legible modern sans-serif.

Prioritize:

- strong heading hierarchy
- large task titles
- readable body text
- clear metadata
- generous line height
- comfortable mobile sizing

Avoid tiny text.

Accessibility is more important than fitting more information onto a screen.

---

## Spacing and Layout

Prefer:

- generous padding
- clear sections
- large touch targets
- predictable spacing
- consistent radius
- progressive disclosure

The Today screen should be understandable in approximately three seconds.

---

## Primary UX Rule

Every major screen should have one dominant action.

Examples:

Today:

```txt
START
```

Brain Dump:

```txt
ORGANIZE THIS
```

Task Breakdown:

```txt
START THIS STEP
```

Focus:

```txt
COMPLETE
```

Rescue:

```txt
START WITH THE MOST IMPORTANT
```

Avoid five equally prominent primary buttons.

---

## Today Screen Rules

The Today screen is the most important surface in the product.

Its primary question is:

> What should I do right now?

Prioritize:

1. energy state
2. THE THING
3. next action
4. secondary tasks
5. optional tasks

Do not show an overwhelming task list by default.

---

## Brain Dump Rules

Brain Dump should require almost zero structure from the user.

Support:

- text
- voice
- pasted text

The user should be able to dump thoughts without categorizing them.

AI can then transform them into structured actions.

Do not require the user to manually set:

- project
- category
- priority
- due date
- tags

before allowing capture.

---

## Task Breakdown Rules

Large tasks should be broken into executable actions.

Good:

```txt
Open the project
Find the auth route
Add the first form field
Run the app
```

Bad:

```txt
Complete authentication implementation
```

Prefer actions that can be started immediately.

Do not decompose infinitely.

Stop when the next action becomes clear and reasonably small.

---

## Focus Mode Rules

Focus Mode should reduce cognitive noise.

Keep the screen minimal.

Show:

- current task
- next action
- timer
- pause
- complete
- I'm Stuck

Avoid dashboards, charts, large statistics, or distracting controls during a focus session.

---

## I'm Stuck Rules

"I'm Stuck" is a core product flow.

Provide simple options such as:

- too much
- don't know where to start
- boring
- low energy
- distracted
- avoiding it

The selected reason should change the AI intervention.

The interaction should be short and actionable.

Never produce a long motivational speech.

---

## Rescue My Day Rules

When a user is overwhelmed:

1. Stop adding work.
2. Identify what truly matters.
3. Remove low-value tasks from today's active plan.
4. Preserve them for later.
5. Give the user one clear next action.

Do not delete tasks just because they were not selected for today.

---

## Scheduling Rules

The scheduling engine should consider:

- calendar events
- available time
- transition buffers
- task duration
- energy
- deadlines
- user preferences
- historical completion behavior

Avoid packing every free minute.

Free time can remain free.

---

## Time Estimation

Treat AI estimates as estimates, not facts.

Track:

- estimated duration
- actual duration
- deviation
- reschedule count

Use behavioral history to improve future estimates.

Never shame the user for inaccurate estimates.

---

## Rescheduling Rules

Unfinished tasks are not failures.

When a task is not completed:

- determine whether it is still important
- consider why it was not completed
- adapt the task if possible
- move it to a realistic future slot
- preserve context

Do not create an endless overdue list.

---

## Notifications

Notifications should be useful nudges, not guilt.

Good:

```txt
Your focus window starts in 10 minutes.
Want to start the proposal?
```

Bad:

```txt
You still haven't completed your task.
```

Prefer fewer intelligent notifications over many generic reminders.

---

## Calendar Integration

Use device/calendar APIs where appropriate.

Separate:

- fixed calendar events
- AI-generated schedule blocks

Never silently overwrite the user's calendar.

Require explicit permission before accessing calendar data.

Clearly communicate what Flowday is scheduling versus what already exists.

---

## Permissions

Request permissions just before they are needed.

Examples:

- notifications → when enabling reminders
- calendar → when user enables calendar planning
- microphone → when user uses voice capture

Do not request every permission during the first app launch.

Explain why each permission is useful.

---

## Analytics Rules

Use PostHog for product analytics.

Track behavioral events such as:

```txt
brain_dump_created
plan_generated
task_started
task_completed
task_rescheduled
task_abandoned
focus_started
focus_completed
stuck_pressed
rescue_day_used
energy_selected
reflection_completed
notification_opened
```

Do not send raw private user content to analytics.

Do not use analytics as an authorization system.

---

## Error Monitoring

Use Sentry.

Capture:

- crashes
- unexpected exceptions
- failed API calls
- native failures
- important AI failures

Include useful debugging metadata but do not leak private user content.

---

## Payments

Use RevenueCat for subscriptions.

Do not implement StoreKit/Google Play billing logic manually unless there is a specific requirement.

Entitlements must be checked server-side when access is security-sensitive.

Do not trust a client boolean like:

```ts
isPremium: true
```

for protected backend functionality.

---

## Environment Variables

Never hardcode secrets.

Use environment variables for:

- Clerk secret keys
- Supabase service keys
- AI provider keys
- PostHog secrets when required
- Sentry secrets when required
- RevenueCat secret/backend credentials

Public client identifiers are different from server secrets.

Never commit `.env` files containing real credentials.

---

## API and Secret Rules

Never expose:

- service role keys
- provider secret keys
- private API credentials
- webhook secrets

to the mobile application.

AI calls requiring secret credentials must go through secure server-side infrastructure.

---

## Images and Assets

Centralize image imports.

Preferred:

```txt
assets/
  images/
    onboarding/
    icons/
    illustrations/
```

Create:

```txt
constants/images.ts
```

when the project benefits from centralized asset access.

Example:

```ts
import onboarding from "@/assets/images/onboarding.png";

export const images = {
  onboarding,
};
```

Do not scatter raw asset paths throughout the app unnecessarily.

---

## Image Generation

When the product explicitly uses generated visual assets:

- keep assets consistent with the Flowday design system
- keep naming organized
- store them in the appropriate assets directory
- avoid unnecessary decorative imagery

Do not introduce large illustrations everywhere.

The product is primarily a productivity interface.

---

## UI Reference Rules

When the user provides a design reference:

The implementation should match it as closely as technically possible.

Match:

- layout
- spacing
- typography
- colors
- borders
- radii
- shadows
- alignment
- component proportions
- interaction states

Do not redesign the reference unless asked.

If a reference conflicts with Flowday's design system, follow the most recent explicit user instruction.

---

## Accessibility

Always consider:

- readable font sizes
- sufficient contrast
- large touch targets
- screen reader labels
- reduced motion
- dynamic text sizing
- focus order

Do not communicate meaning through color alone.

---

## Animation Rules

Use animation to provide:

- continuity
- feedback
- state changes
- progress

Avoid animation that:

- delays the user
- distracts during focus
- creates visual noise
- reduces accessibility

Keep motion subtle and purposeful.

---

## Performance

Prioritize:

- fast initial render
- low unnecessary re-renders
- stable list keys
- virtualized long lists
- efficient image loading
- memoization only when useful
- minimal network requests

Do not optimize blindly.

Measure before introducing complexity.

---

## Navigation Rules

Use Expo Router.

Keep routes predictable.

Prefer route groups for major app states:

```txt
(auth)
(tabs)
```

Do not put large state machines inside navigation definitions.

Navigation should represent user flow, not business logic.

---

## TypeScript Rules

Use TypeScript strictly.

Avoid `any`.

Prefer:

- explicit interfaces/types
- discriminated unions
- typed API responses
- typed navigation params
- schema-derived types where practical

Do not create giant type abstractions that make simple code difficult to read.

---

## Validation Rules

Validate inputs at boundaries.

Examples:

- forms
- API requests
- AI outputs
- route parameters
- database writes
- subscription state

Use a shared validation package where appropriate.

---

## Error Handling

Never silently swallow errors.

Good:

```ts
try {
  await saveTask();
} catch (error) {
  reportError(error);
  showUserFriendlyMessage();
}
```

Bad:

```ts
try {
  await saveTask();
} catch {}
```

User-facing errors should be understandable.

Developer errors should contain enough context to diagnose the issue.

---

## Logging

Use structured logging where appropriate.

Do not use noisy `console.log` statements throughout production code.

Never log:

- auth tokens
- API keys
- passwords
- private notes
- private AI conversations
- sensitive calendar information

---

## Testing Philosophy

For important product behavior, test the logic rather than only the UI.

Prioritize tests for:

- daily planning
- task prioritization
- task breakdown
- rescheduling
- rescue-day logic
- energy adaptation
- authorization
- subscription entitlement
- API validation

For UI, prioritize critical user journeys.

---

## Feature Implementation Workflow

When the user asks to build a feature:

1. Read this file.
2. Understand the existing architecture.
3. Find the relevant feature/module.
4. Identify the smallest set of files to change.
5. Reuse existing components and utilities.
6. Implement the smallest useful version.
7. Validate types.
8. Run linting.
9. Run tests relevant to the feature.
10. Check the mobile UI.
11. Fix errors before finishing.
12. Explain what changed and how to test it.

Do not rewrite unrelated code.

---

## Do Not Overengineer

Start simple.

Do not introduce:

- microservices
- Kubernetes
- message queues
- multiple databases
- complex event buses
- custom caching infrastructure

unless real product requirements justify them.

A simple reliable architecture is preferred.

---

## Adding Dependencies

Do not add dependencies automatically.

If a dependency is necessary:

1. Explain what problem it solves.
2. Check whether an existing dependency already solves it.
3. Check compatibility with the existing Expo/React Native version.
4. Prefer established libraries.
5. Avoid duplicate libraries with overlapping functionality.

---

## Existing Patterns

Before writing new code:

- search for similar components
- search for similar hooks
- search for existing API helpers
- search for existing validation
- reuse established styles
- follow existing naming

Consistency is more important than personal preference.

---

## File Naming

Prefer clear names.

Examples:

```txt
TaskCard.tsx
EnergySelector.tsx
useDailyPlan.ts
planning.ts
task-schema.ts
calendar-service.ts
```

Avoid:

```txt
Thing.tsx
Utils.ts
Helper.ts
Stuff.ts
NewComponent.tsx
```

---

## Component Naming

Use semantic names.

Good:

```txt
NextActionCard
DailyPlanSection
FocusTimer
EnergySelector
StuckReasonCard
```

Bad:

```txt
Box
Card2
ContainerThing
BlueButton
```

---

## Business Logic Boundaries

UI components should not contain large business rules.

Prefer:

```txt
UI
 ↓
Hook
 ↓
Service / feature logic
 ↓
API
 ↓
Backend
```

Keep planning logic in the planning engine.

Keep authentication in Clerk integration.

Keep persistence in database/service layers.

---

## AI UX Rules

AI should be:

- concise
- contextual
- actionable
- calm
- transparent

Do not make the user read huge AI responses.

Prefer:

```txt
What matters
Why
Next action
Action button
```

over:

```txt
large paragraph of explanation
```

---

## AI Should Not Pretend

Never claim:

- to know the user's emotions with certainty
- to know why a user failed
- to diagnose ADHD
- to provide medical treatment
- to have performed an action it did not perform

Use language like:

- "It looks like..."
- "Based on your plan..."
- "Would you like me to..."
- "I suggest..."

---

## Medical / Mental Health Boundary

Flowday may use ADHD-friendly productivity principles, but it is not a medical device or diagnostic system.

Do not present the app as:

- treating ADHD
- diagnosing ADHD
- replacing professional care
- providing medical advice

Keep the product focused on productivity and executive-function support.

---

## Security

Treat all client input as untrusted.

Validate:

- IDs
- ownership
- permissions
- payloads
- AI outputs

Do not trust:

- client-provided user IDs
- client subscription state
- client-generated ownership fields
- unvalidated AI instructions

---

## Release Quality

Before considering a feature complete, verify:

### Functionality

- happy path works
- errors are handled
- loading state exists
- empty state exists
- retry state exists where relevant

### UX

- keyboard behavior works
- safe area is correct
- touch targets are large enough
- navigation makes sense
- loading does not feel stuck

### Backend

- authentication works
- authorization works
- schema validation works
- database ownership is enforced

### AI

- structured output validates
- model failure has fallback behavior
- secrets stay server-side

---

## Linting and Validation

Run the project's actual scripts.

Typical commands may include:

```bash
pnpm lint
pnpm typecheck
pnpm test
```

Do not assume exact script names if `package.json` differs.

Before finishing a meaningful feature:

- fix TypeScript errors
- fix lint errors
- run relevant tests
- confirm build compatibility

---

## Git Rules

Keep commits focused.

Avoid huge commits containing unrelated refactors.

Prefer commit intent such as:

```txt
feat: add adaptive today planner
feat: add stuck flow
fix: prevent duplicate daily plans
refactor: extract task card
```

Do not rewrite git history unless explicitly requested.

---

## Communication Style

When reporting work:

1. State what changed.
2. State the important architectural decision.
3. State how to test it.
4. Mention any remaining limitation.

Be concise and concrete.

Avoid unnecessary explanations of obvious implementation details.

---

## Final Checklist Before Every Feature

Before finishing any feature, ask:

- Did I read AGENTS.md?
- Did I follow the existing architecture?
- Is the feature solving the actual user problem?
- Is there one obvious primary action?
- Is the UI calm and low-cognitive-load?
- Did I reuse existing components?
- Did I avoid unnecessary dependencies?
- Are auth and secrets handled correctly?
- Are backend inputs validated?
- Are user-owned records protected?
- Are AI outputs structured and validated?
- Did I handle loading/error/empty states?
- Did I run lint/typecheck/tests?
- Did I verify the mobile UX?

---

## Final Product Principle

Flowday is not a task database with AI added to it.

It is an **adaptive execution companion**.

The application should continuously move the user from:

```txt
Overwhelmed
    ↓
Clear
    ↓
One small action
    ↓
Started
    ↓
Focused
    ↓
Progress
```

When making product or engineering decisions, prefer the solution that gets the user closer to:

> **"I know what to do, and I can start now."**

over the solution that merely gives the user more information.
