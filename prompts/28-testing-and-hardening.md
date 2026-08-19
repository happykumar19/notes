Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Phases 21, 22, and 23.

**Unit tests, on logic rather than UI**, per `docs/AGENTS.md` § Testing Philosophy. Priority order: planning rules (time budget, energy adaptation, priority, task sizing), rescue rules, task lifecycle and time calculations, and AI schema validation against malformed, truncated, empty, and hostile output.

**Integration tests.** Clerk sign in, sign out, account creation, and session restoration. Database authorization, proving a user cannot read or mutate another user's rows across every user-owned table. The full task lifecycle. Plan generation with realistic context.

**End-to-end journeys.** New user through onboarding to first plan. Today to Start to Focus to Complete. Today to I'm Stuck to intervention to resume. Overloaded day to rescue to updated plan. End of day to reflection to updated patterns.

**Security audit**, Phase 22.1. No secrets committed. Clerk client and server boundaries correct. RLS on every user-owned table. Every protected mutation checks ownership. AI provider credentials never reachable from the client. Verify by inspecting an actual production bundle, not by reading the source.

**Privacy audit**, Phase 22.2. Analytics payloads carry no private text. Logs carry no task, calendar, or AI content. Retention and deletion rules defined. Account deletion actually removes associated data everywhere, including Supabase rows, RevenueCat, PostHog, and any embeddings.

**Performance**, Phase 23.1. Measure cold start and first navigation before optimizing anything. Then Today rendering, long task lists, and asset sizes. `docs/AGENTS.md` says measure before adding complexity, and that applies here more than anywhere.

**Polish**, Phase 23.2. Every loading, error, and empty state across the app, each with a recovery or next action. Subtle motion on task completion, plan changes, focus transitions, and bottom sheets, all respecting reduced motion.

**Accessibility**, Task 23.9. Font scaling at the largest Dynamic Type sizes, screen reader labels and focus order, contrast, and 44x44 touch targets. Then verify the rule that this app breaks most easily: nothing communicates meaning through color alone. Check the energy chips, priority tiers, task status, and the schedule's fixed-versus-generated distinction specifically.

Run the full checklist in `docs/AGENTS.md` § Release Quality and report what fails rather than fixing it silently.
