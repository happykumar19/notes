Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 3.4-3.10, 4.3, 4.4, and the offline behavior in `docs/AGENTS.md` § Offline / Optimistic UX.

Build the server access layer and the client state layer for tasks. This is the plumbing every later feature depends on, so get the boundaries right.

Server side, using Expo API routes:

- A task service that reads and writes through the authenticated Clerk session, never a client-supplied id.
- Validation schemas for create, update, complete, and reschedule. Validate at the boundary, before touching the database.
- Endpoints: create, read, update, delete, complete, reschedule.
- Every endpoint checks authentication, then ownership, then payload, in that order, before doing anything.

Client side:

- TanStack Query provider with sensible stale times for a mobile app that backgrounds constantly.
- Hooks: `useTasks`, `useTask`, `useCreateTask`, `useUpdateTask`, `useCompleteTask`, `useRescheduleTask`.
- Zustand for client-only state: selected energy state, active focus session, transient planning UI. Zustand is not a cache for server data.
- AsyncStorage for small local persistence only. No tokens, no secrets, no large datasets.

Task completion is optimistic and non-negotiable: the checkmark lands instantly, the mutation syncs behind it, and a failure rolls back with a quiet retry rather than an error dialog. A user tapping "done" must never wait on the network.

Also finish onboarding persistence, Tasks 4.3 and 4.4: save the onboarding answers collected in prompt `05` to `user_preferences`, mark onboarding complete, and route new users to their first energy check-in while returning users go straight to Today.

Build the basic task UI from Task 3.10 against the `tasks` screen design so the layer is provably working end to end.

State which dependencies you are adding and why before installing them.
