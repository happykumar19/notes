Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 14.1-14.7.

Personalization is the product's differentiator, so build it deliberately and no larger than it needs to be.

Separate memory into four categories, per Task 14.1: stated preferences, stable patterns, temporary context, and planning behavior. They have different lifetimes and different trust levels, and collapsing them into one store is what makes AI memory feel wrong.

Define retention rules before writing anything: what expires, what is aggregated and then discarded, and what is never stored at all. Per `docs/AGENTS.md` § Personal Memory, store useful behavioral patterns such as preferred focus windows, estimation bias, preferred task size, rescheduling patterns, energy patterns, and planning density. Do not store raw personal notes or private calendar content.

Read `docs/AGENTS.md` § Personal Memory before enabling pgvector: "Use pgvector only when semantic memory actually provides value. Do not introduce embeddings everywhere just because they are available." The structured patterns from prompt `22` already cover most planning needs. Before adding embeddings, state the specific retrieval problem structured fields cannot solve. If there isn't one, skip Tasks 14.3 to 14.5 and say so.

If it is justified: enable pgvector, embed only approved memory records, and build retrieval that returns a small relevant set into `PlanningContext` rather than everything.

Task 14.7 is a requirement, not a nice-to-have. The user can see every stored memory in plain language, correct it, and delete it. An AI that has silently decided something wrong about how you work, with no way to fix it, is worse than an AI with no memory. Put this in settings and make it easy to find.

Memory influences suggestions. It never silently overrides an explicit user choice.
