Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 18.1-18.7.

There is no generated paywall design. Build it from `DESIGN.md` and show me before shipping it.

Define the tiers first, and be specific rather than aspirational. `docs/todo.md` Task 18.2 suggests Pro includes unlimited AI, adaptive scheduling, advanced insights, memory, voice, and advanced calendar planning. Whatever the split, the free tier must let a user complete the core loop at least once a day — capture, plan, start, focus, complete. A user who cannot experience the product will not pay for it.

Integrate RevenueCat for products and entitlements. Do not write StoreKit or Play Billing code by hand.

Read entitlement state in the app for UI gating, and check it server-side on every AI endpoint, since those are the ones with real cost. `docs/AGENTS.md` is explicit: a client boolean like `isPremium: true` is never sufficient for protected backend functionality.

The paywall explains value without dark patterns. No fake countdowns, no manufactured scarcity, no pre-selected annual plan disguised as monthly, no hidden dismiss control. This product's users are specifically people who struggle with impulse decisions, and manipulating that is both wrong and a refund generator.

Restore purchases must be present, reachable from settings without signing in again, and actually tested on both stores.

When a free user hits a limit, say plainly what the limit is and when it resets. Never break mid-flow: if a plan is being generated, finish it, then explain. Never lose user data at a paywall boundary.
