Read `docs/AGENTS.md`, `prompts/AGENTS.md`, and `prompts/SCREEN-MAP.md` first and follow them strictly.

Covers `docs/todo.md` Task 2.6, UI only.

**There is no generated design for these screens.** Derive them from `flowday_adaptive_harmony/DESIGN.md` and the visual language of `onboarding_welcome` and `onboarding_ready`. Show me the result before moving on.

Build Sign Up and Sign In in `(auth)` with mocked behavior. Real Clerk wiring is prompt `07`.

Both screens share one layout: the Flowday logo mark, a headline in `display-lg-mobile`, email input using the bottom-border-only field style from `DESIGN.md` that turns sage on focus, one pill primary button, social auth buttons for Apple and Google, and a link to the other screen.

Sign Up additionally links to terms and privacy. Neither screen has a password field; authentication is email code plus social.

Verification modal, triggered by the primary button on either screen:

- Copy says a code was emailed.
- Six digits, number pad keyboard.
- Stays above the keyboard on both platforms.
- Auto-submits when the sixth digit is entered.
- Has a resend action with a visible cooldown.
- Has visible error state for a wrong code that does not clear the whole field.

On success, navigate to `(tabs)/today`.

Update the last onboarding screen so its primary action opens Sign Up.

Error copy stays plain and blameless. "That code didn't work. Want a new one?" not "Invalid credentials."
