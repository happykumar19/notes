Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 2.1-2.3, 2.8-2.11, and 3.1-3.3.

Set up Supabase and the full core schema in one pass, so later prompts add features instead of migrating identity.

Create `supabase/` at the repository root with versioned migrations. Every schema change from here on is a migration file. Never hand-edit a deployed schema.

Tables, using the names mandated by `docs/AGENTS.md` § Database Rules:

```txt
users              clerk_user_id as external identity, internal uuid as primary key
user_preferences   preferred structure, focus duration, notification prefs, planning prefs
projects
tasks              title, notes, status, priority, due date, estimated duration,
                   actual duration, completed_at, reschedule_count, energy_required
task_steps         ordered breakdown steps with their own completion state
energy_checkins    state enum + timestamp
```

Every user-owned table carries the internal user id and has Row Level Security enabled with an explicit ownership policy. Then prove it: write a test that a second user cannot read or mutate the first user's rows. Task 2.11 is not done until that test passes.

Clerk and Supabase must be bridged deliberately. Decide and document one approach: either Clerk-issued JWTs verified by Supabase so RLS reads the Clerk subject, or a server-side layer that maps the verified Clerk session to the internal user id. Do not accept a user id sent by the client under any circumstances.

Also build the bootstrap flow from Task 2.9: on first authentication, create or fetch the user profile, seed default preferences, and initialize onboarding state. It must be idempotent, because it will run on every cold start.

Generate TypeScript types from the schema so the app never hand-writes row types.

Secrets: the anon key may ship in the client. The service role key never leaves the server. Add `.env.example` with placeholder values and confirm real `.env` files are gitignored.
