Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 4.1 and 4.2, UI only.

Design references, in order:

```txt
assets/stitch_flowday_executive_productivity_companion/onboarding_welcome/
assets/stitch_flowday_executive_productivity_companion/onboarding_purpose/
assets/stitch_flowday_executive_productivity_companion/onboarding_challenges/
assets/stitch_flowday_executive_productivity_companion/onboarding_structure/
assets/stitch_flowday_executive_productivity_companion/onboarding_peak_focus/
assets/stitch_flowday_executive_productivity_companion/onboarding_ready/
```

Build the six-screen onboarding sequence in `(onboarding)`.

**Replace the existing welcome screen.** `flowstate/src/screens/onboarding/` was built from a different reference — a `welcome-hero` photo with a feature-row list — and does not match `onboarding_welcome`, which is a full-bleed composition with organic sage and amber shapes and a centered logo mark. Rebuild it from the Stitch design.

Reuse what still applies: the `FeatureRow` component if any screen in the new sequence needs it, the safe-area and scroll conventions, and the theme tokens. Delete what does not, including the now-unused `welcome-hero` asset and its entry in `constants/images.ts`. Do not leave dead assets behind.

Behavior:

- The three question screens (challenges, structure, peak focus) collect the onboarding state named in Task 4.1: common blockers, preferred structure, focus preferences, goals, energy patterns. Type this state now, in `src/types/`, because Task 4.3 persists it and Phase 7 planning consumes it.
- Hold answers in local component state for now. Persistence arrives in prompt `09`.
- Every screen has one dominant forward action. Back is always available and never loses answers.
- Selection states use the 3px sage inner border from `DESIGN.md`.
- No pagination dots.
- Copy must stay non-clinical. This asks about work habits, not symptoms. Nothing on these screens may read as a diagnostic questionnaire.

The final screen hands off to sign-up. Wire that navigation once prompt `06` exists; for now route to a placeholder.
