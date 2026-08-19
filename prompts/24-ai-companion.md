Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 15.1-15.5.

Design reference: `assets/stitch_flowday_executive_productivity_companion/ai_companion_1/`

Do not use `ai_companion_2`. It belongs to the rejected five-tab family and offers only three of the six actions.

The companion opens from the floating action button, not from a tab. Its six quick actions match Task 15.1 exactly: What should I do now, Rescue my day, Break down a task, Plan tomorrow, I'm stuck, Brain dump.

Build an action router. Each quick action maps to an existing validated operation from earlier prompts, not to a fresh model call. "Rescue my day" runs the rescue service from prompt `18`. "Break down a task" runs the decomposition from prompt `15`. The companion is a front door to the app's real capabilities, not a parallel implementation of them.

Responses follow the schema in Task 15.2: context, recommendation, next action, action button. The `ai_companion_1` design shows the target shape — a short factual read of the situation, a concrete proposal, then `Do it` and `Review`.

`docs/AGENTS.md` § AI UX Rules caps the length: what matters, why, next action, button. Never a long paragraph. A user who opens this is looking for a decision, not a conversation.

When the companion proposes a change, `Do it` executes it through the validated service with ownership and schema checks intact, and `Review` shows exactly what will change first. The AI never mutates data on its own initiative.

Free-text input routes to the closest supported action rather than becoming an open-ended chatbot. If nothing matches, say so plainly and offer the quick actions. `docs/AGENTS.md` § AI Should Not Pretend applies to every response: no claiming to know the user's emotions, no diagnosing, no claiming to have done something it did not do.

Use the companion avatar from `professional_but_friendly_3d_avatar_headshot_of_a_supportive_ai_companion._soft` consistently with the AI cards elsewhere in the app.

Companion conversations are private. They are never sent to analytics and never written to logs.
