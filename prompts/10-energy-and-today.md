Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 5.1-5.8.

Design reference: `assets/stitch_flowday_executive_productivity_companion/today/` — use `screen.png` for layout and `code.html` for spacing and copy. Borrow only two things from `today_flowday_1`: the secondary `Break it down` action next to `START`, and the BONUS placeholder row.

This is the most important screen in the product. `docs/AGENTS.md` § Today Screen Rules and § Primary UX Rule both apply in full.

Energy check-in:

- Persist to `energy_checkins`. State plus timestamp.
- The `EnergySelector` chips from prompt `03` sit inline at the top of Today, exactly as in the design.
- Expose the current energy state through a planning context API, because Phase 7 consumes it. Design that API now even though nothing reads it yet.

Today screen, in the order the design shows and the roadmap requires:

1. Greeting and available time
2. Energy chips
3. The AI window card
4. THE THING, with one dominant `Start Focus`
5. WOULD BE NICE
6. BONUS
7. The stuck / brain dump entry point

Data contract from Task 5.5, typed and shared:

```ts
{ theThing, wouldBeNice[], bonus[], nextAction, availableTime }
```

Populate it from real tasks using simple deterministic selection. No AI in this prompt. Prompt `14` replaces the selection logic behind this same contract, so keep the contract stable and the selection swappable.

Tapping the primary action starts the task. Wire it to the focus route once prompt `16` exists; until then navigate to the placeholder.

Three states that must all exist and all feel intentional: no tasks at all, tasks but no plan yet, and plan loading. `docs/AGENTS.md` says this screen is understandable in about three seconds, and that includes the empty state.

Never render a flat list of every task here.
