Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 6.4-6.7 and the rules in `docs/AGENTS.md` § Backend Architecture, § AI Output Rules, § AI Provider Rules, and § AI Prompt Rules.

Build the server-side AI gateway. This is the single choke point for every model call in the product, so its shape matters more than the first feature that uses it.

Gateway, server-side only:

- One internal interface for all model calls. No screen and no client hook ever calls a provider.
- Model tiers behind that interface: fast, reasoning, and a fallback provider. Callers ask for a tier and a task, not for a vendor or a model name.
- Provider-specific code exists in exactly one adapter file per provider.
- Timeouts, one bounded retry, and a fallback path when a provider fails or returns garbage.
- Structured logging of latency, tier, outcome, and token cost. Never log the prompt content or the user's text.

Prompts live in a versioned server-side module with a version string attached to every call, per `docs/AGENTS.md` § AI Prompt Rules. Never inline a prompt in a screen or a route file.

The mandatory pipeline, from `docs/AGENTS.md`, applies to every call without exception:

```txt
request → context builder → prompt → provider → structured JSON → schema validation → business rules → persist → response
```

First consumer, brain-dump extraction:

- Output schema: `actionableTasks[]`, `ideas[]`, `reminders[]`, `laterItems[]`, each item carrying a title, an optional estimate, and a confidence.
- Use the fast tier. This is extraction, not reasoning.
- The prompt turns unstructured thought into units the user can start, per `docs/AGENTS.md` § Task Breakdown Rules. "Complete authentication implementation" is a failure; "Open the project and find the auth route" is the target.
- Validate at runtime: required fields, enum values, array size limits, duration bounds. Never trust that the model returned valid data.
- On invalid output, retry once, then fall back to line-splitting the raw text into unclassified items. The user must never lose their brain dump because a model misbehaved, and the app must never crash on bad AI output.

Provider keys are server-side environment variables. They never reach the Expo app in any form.

Write tests against malformed, truncated, empty, and hostile model output. Task 21.4 depends on this being testable from the start.
