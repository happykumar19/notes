Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 1.5 and 1.6.

Design source: `assets/stitch_flowday_executive_productivity_companion/flowday_adaptive_harmony/DESIGN.md`

The token files in `flowstate/src/theme/` (`colors`, `fonts`, `typography`, `spacing`, `radius`, `shadows`, `motion`) already exist and Hanken Grotesk already loads through the `expo-font` config plugin. Complete and correct that system rather than rebuilding it.

Reconcile every token against `DESIGN.md` and make sure each is reachable both as a Tailwind utility in `global.css` and as a typed export from `@/theme`:

- Colors: the full Material-style role set, plus the AI slate blue `#457B9D` and its 10% tint.
- Typography: `display-lg`, `display-lg-mobile`, `headline-md`, `title-task`, `body-main`, `label-metadata`, `label-caps`. Line heights of 1.5x and above are mandatory on body and metadata.
- Radius: cards 24px, buttons and chips fully pill, plus the `sm`/`DEFAULT`/`md`/`lg`/`xl`/`full` scale.
- Spacing: `space-xs` through `space-xl`, `safe-margin` at 20px, `gutter`.
- Elevation, exactly as specified in `DESIGN.md`: base cream; level 1 cards `0px 4px 20px rgba(26,26,26,0.04)`; level 2 floating adds `0px 8px 30px rgba(26,26,26,0.08)`. Pressed cards sink, meaning the shadow reduces or disappears.

Two things `DESIGN.md` states that must be encoded as tokens, not left to individual screens:

- The AI suggestion bubble shape: 32px radius with one 8px corner.
- The selection state: 3px inner border in primary sage, keeping the 24px radius.

Shadows differ between iOS and Android. Use the `StyleSheet` escape hatch from `docs/AGENTS.md` § Style Exceptions where `className` cannot express it, and keep the platform handling inside the theme layer so screens never branch on platform.

Support Dynamic Type scaling and verify the type scale stays readable at large accessibility sizes.

Do not build screens or components in this prompt.
