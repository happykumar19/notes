# Screen Map

Canonical mapping from generated Stitch screens to Flowday features, routes, and roadmap tasks.

Base path for every reference below:

```txt
assets/stitch_flowday_executive_productivity_companion/
```

---

## The variant rule

The generated screens split into two incompatible families:

| Family | Tab bar | Screens |
| --- | --- | --- |
| **Use this one** | 4 tabs: Today, Tasks, Focus, Insights. AI companion via FAB. | `today`, `brain_dump_input_1`, `focus_mode_active_1`, `ai_companion_1`, `insights_1`, `tasks`, `schedule`, `brain_dump_organized`, `break_this_down_portfolio_website`, `evening_reflection_1`, `focus_mode_completed`, `today_flowday_1` |
| **Do not use** | 5 tabs with a dedicated `AI` tab. | `today_flowday_2`, `brain_dump_input_2`, `focus_mode_active_2`, `ai_companion_2` |

The 4-tab family matches `docs/todo.md` Task 1.9. Mixing families produces two different navigation
shells in one app.

---

## Screen assignments

| Feature | Route | Use | Notes |
| --- | --- | --- | --- |
| Onboarding: welcome | `/(onboarding)/welcome` | `onboarding_welcome` | Full-bleed sage/amber organic shapes, centered logo mark. Note: the current `src/screens/onboarding` was built from a different reference and does not match this. |
| Onboarding: purpose | `/(onboarding)/purpose` | `onboarding_purpose` | |
| Onboarding: challenges | `/(onboarding)/challenges` | `onboarding_challenges` | Feeds `docs/todo.md` Task 4.1 "common blockers". |
| Onboarding: structure | `/(onboarding)/structure` | `onboarding_structure` | Feeds "preferred structure". |
| Onboarding: peak focus | `/(onboarding)/peak-focus` | `onboarding_peak_focus` | Feeds "energy patterns" + `user_patterns.best_focus_hours` seed. |
| Onboarding: ready | `/(onboarding)/ready` | `onboarding_ready` | Hands off to first energy check-in. |
| Sign up / Sign in | `/(auth)/sign-up`, `/(auth)/sign-in` | **No design exists** | Derive from `DESIGN.md` + the onboarding screens. See prompt `06`. |
| Today | `/(tabs)/today` | **`today`** | Named energy chips (all 5 states), THE THING, WOULD BE NICE, AI window card, stuck/brain-dump entry, one dominant `Start Focus`. Borrow the secondary `Break it down` action and the BONUS placeholder row from `today_flowday_1`. |
| Energy check-in | inline on Today | `today` (chip row) | Chips carry labels, not just color. Keep it that way. |
| Tasks | `/(tabs)/tasks` | `tasks` | |
| Task detail | modal `/task/[id]` | **`task_detail_auth_refactor`** | Modal chrome (back arrow, no tab bar), matching `docs/todo.md` Task 1.10. `task_detail_client_proposal` is the same screen with a tab bar; use it only as a secondary copy reference. |
| Task breakdown | modal `/task/[id]/breakdown` | `break_this_down_portfolio_website` | |
| Brain dump: capture | modal `/brain-dump` | **`brain_dump_input_1`** | Large free-form field, `Use voice instead`, one dominant `Organize this`. |
| Brain dump: review | modal `/brain-dump/review` | `brain_dump_organized` | The edit-before-import step, Task 6.8-6.10. |
| Focus mode | full-screen `/focus` | **`focus`** | Dark immersive, no tab bar, ring timer, pause + `Complete Task`. `focus_animated_deep_flow` is the identical screen with an animation token; use it as the motion reference. Add an `I'm Stuck` secondary action, which `focus` is missing and `docs/AGENTS.md` § Focus Mode Rules requires. Borrow the `Next Micro-Step` row from `focus_mode_active_2`. |
| Focus complete | `/focus/complete` | `focus_mode_completed` | |
| I'm Stuck: reason | modal `/stuck` | `i_m_stuck_what_s_wrong` | Six reasons, exactly matching Task 8.6. Plus a `Just tell me what to do` escape. |
| I'm Stuck: intervention | modal `/stuck/[reason]` | `i_m_stuck_too_much`, `i_m_stuck_start_small` | Two of the six interventions exist. The other four must be designed to match. |
| Rescue My Day | modal `/rescue` | `rescue_my_day` | MUST HAPPEN / SHOULD HAPPEN / CAN WAIT maps to `mustHappen[]` / `shouldHappen[]` / `canWait[]` in Task 10.2. Keep the "Nothing was deleted" footer. |
| Schedule | modal `/schedule` | `schedule` | Must visually separate fixed calendar events from AI-generated blocks. |
| Evening reflection | modal `/reflection` | **`evening_reflection_2`** | Modal chrome matches Task 1.10. Take the three-card wrap-up and the `Adaptive Learning` copy from `evening_reflection_1`. |
| Insights | `/(tabs)/insights` | **`insights_1`** | Correct tab shell. `insights_2` is the content model for Phase 13: Peak Performance, Planning vs Reality, Less is more. Build `insights_1` first, then add `insights_2` cards. |
| AI companion | modal `/companion` | **`ai_companion_1`** | All six quick actions match Task 15.1 exactly. |
| Notifications | `/settings/notifications` | `notifications_nudges` | |
| Home screen widgets | native widget | `home_screen_widgets` | Phase 16. |
| Brand logo | app icon, splash, headers | `flowday_brand_logo` | Currently unused; the app still ships the Expo default icon. |
| AI companion avatar | companion + AI cards | `professional_but_friendly_3d_avatar_headshot_of_a_supportive_ai_companion._soft` | The face that appears in AI bubbles across Today, Focus, and Reflection. |
| Shaders | Focus mode background | `shader_1`, `shader_2` | Optional. Skip unless Focus mode needs ambient motion. Respect reduced-motion. |

---

## Missing designs

These have roadmap tasks but no generated screen. They need to be designed from `DESIGN.md`
before their prompt runs:

- Sign in / Sign up / verification code
- Settings and account
- Paywall (Task 18.5)
- Four of the six I'm Stuck interventions
- Empty states for Today, Tasks, and Insights
- Error and offline states
