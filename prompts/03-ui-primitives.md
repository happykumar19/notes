Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 1.7 and 1.8.

Build the shared component library in `flowstate/src/components/`, driven entirely by the tokens from prompt `02`. No hardcoded colors, sizes, or shadows.

Generic primitives:

- `PrimaryButton`, `SecondaryButton` — pill-shaped, full width by default, with pressed, disabled, and loading states
- `Card` — 24px radius, level 1 elevation, sinks on press when interactive
- `IconButton`, `Badge`, `Divider`
- `EmptyState`, `LoadingState`, `ErrorState` — every one needs a recovery or next action, never a dead end
- `BottomSheet`

Flowday primitives, each derived from a real screen so the API matches actual usage:

- `EnergySelector` — the five states from `docs/AGENTS.md`: Deep Focus, Normal, Scattered, Low Energy, Overwhelmed. Reference `today`. Every chip shows its label; color alone is never the signal.
- `PriorityCard` — three variants per `DESIGN.md`: `theThing` has a sage border and larger title, `wouldBeNice` is standard, `bonus` is dashed or reduced opacity. Reference `today` and `today_flowday_1`.
- `TaskCard` — reference `tasks`. Includes an energy indicator and a duration estimate.
- `NextActionCard` — the single "what do I do right now" surface.
- `AIInsightCard` — the asymmetric bubble shape from `DESIGN.md`, using the companion avatar from `professional_but_friendly_3d_avatar_headshot_of_a_supportive_ai_companion._soft`. Reference the green window card in `today` and the blue card in `today_flowday_1`.
- `StuckReasonCard` — reference `i_m_stuck_what_s_wrong`.
- `FocusTimer` — the ring timer from `focus`. Elapsed time comes from timestamps, never from frame counting.
- `ProgressIndicator`

Every component needs an accessibility label, a touch target of at least 44x44, and support for reduced motion.

Build only what these screens actually use. Do not create wrapper components that add no meaning, and do not build a Storybook.
