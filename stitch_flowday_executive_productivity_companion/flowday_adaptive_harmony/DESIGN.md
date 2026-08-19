---
name: Flowday Adaptive Harmony
colors:
  surface: '#fcf9f8'
  surface-dim: '#dcd9d9'
  surface-bright: '#fcf9f8'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f6f3f2'
  surface-container: '#f0eded'
  surface-container-high: '#eae7e7'
  surface-container-highest: '#e5e2e1'
  on-surface: '#1c1b1b'
  on-surface-variant: '#424842'
  inverse-surface: '#313030'
  inverse-on-surface: '#f3f0ef'
  outline: '#737972'
  outline-variant: '#c2c8c0'
  surface-tint: '#4a654e'
  primary: '#4a654e'
  on-primary: '#ffffff'
  primary-container: '#8ba88e'
  on-primary-container: '#233d29'
  inverse-primary: '#b0ceb2'
  secondary: '#765a05'
  on-secondary: '#ffffff'
  secondary-container: '#ffd87c'
  on-secondary-container: '#795d08'
  tertiary: '#a33d23'
  on-tertiary: '#ffffff'
  tertiary-container: '#f97d5e'
  on-tertiary-container: '#6e1701'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#cceace'
  primary-fixed-dim: '#b0ceb2'
  on-primary-fixed: '#07200f'
  on-primary-fixed-variant: '#334d38'
  secondary-fixed: '#ffdf96'
  secondary-fixed-dim: '#e7c268'
  on-secondary-fixed: '#251a00'
  on-secondary-fixed-variant: '#5a4400'
  tertiary-fixed: '#ffdad2'
  tertiary-fixed-dim: '#ffb4a2'
  on-tertiary-fixed: '#3c0700'
  on-tertiary-fixed-variant: '#83260e'
  background: '#fcf9f8'
  on-background: '#1c1b1b'
  surface-variant: '#e5e2e1'
typography:
  display-lg:
    fontFamily: Hanken Grotesk
    fontSize: 40px
    fontWeight: '700'
    lineHeight: 48px
    letterSpacing: -0.02em
  display-lg-mobile:
    fontFamily: Hanken Grotesk
    fontSize: 32px
    fontWeight: '700'
    lineHeight: 38px
    letterSpacing: -0.01em
  headline-md:
    fontFamily: Hanken Grotesk
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
  title-task:
    fontFamily: Hanken Grotesk
    fontSize: 18px
    fontWeight: '500'
    lineHeight: 24px
  body-main:
    fontFamily: Hanken Grotesk
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 26px
  label-metadata:
    fontFamily: Hanken Grotesk
    fontSize: 13px
    fontWeight: '600'
    lineHeight: 18px
    letterSpacing: 0.02em
  label-caps:
    fontFamily: Hanken Grotesk
    fontSize: 11px
    fontWeight: '700'
    lineHeight: 16px
    letterSpacing: 0.05em
rounded:
  sm: 0.5rem
  DEFAULT: 1rem
  md: 1.5rem
  lg: 2rem
  xl: 3rem
  full: 9999px
spacing:
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2.5rem
  safe-margin: 1.25rem
  gutter: 1rem
---

## Brand & Style
The design system focuses on a **premium, supportive, and human-centric** experience. It moves away from the anxiety-inducing "hustle culture" of traditional productivity apps, instead favoring a calm, intentional pace. The style is a blend of **Minimalism** and **Tactile Modernism**, prioritizing cognitive ease and emotional regulation through a soft, warm aesthetic.

The visual narrative is built on the concept of "Gentle Focus." This is achieved through:
- **Generous Whitespace:** Providing mental breathing room between tasks.
- **Organic Softness:** Utilizing high-radius corners and fluid transitions to mimic natural, non-threatening shapes.
- **Intentional Depth:** Using subtle, low-blur shadows to create a sense of physical layers that are easy for the eye to parse.

## Colors
The palette is grounded in warm, earthy tones to evoke a sense of stability and calm. 

- **Primary (Muted Sage):** Used for standard progress, positive states, and primary actions. It represents growth and focus without the clinical feel of standard blue.
- **Secondary (Soft Amber):** Reserved for "Important" but non-urgent tasks. It provides a warm glow of attention.
- **Tertiary (Muted Coral):** Used sparingly for urgency or high-energy requirements. Its muted nature prevents it from feeling alarming.
- **Background (Soft Cream):** The foundation of the UI, reducing eye strain compared to pure white and providing a sophisticated, paper-like warmth.
- **Typography (Charcoal):** Provides high legibility while being softer on the eyes than pure black.

## Typography
**Hanken Grotesk** is the sole typeface, chosen for its contemporary precision and friendly, open counters. 

- **Headlines:** Use large, expressive weights to establish clear hierarchy and a sense of "editorial" quality.
- **Task Titles:** Set in Medium weight to ensure they stand out as the primary interactive element on cards.
- **Body & Metadata:** High line-heights (1.5x+) are mandatory to maintain the "airy" feel of the design system.
- **Scalability:** For mobile, display sizes should scale down to prevent awkward word wrapping, while maintaining a significant contrast against body text.

## Layout & Spacing
This design system utilizes a **contextual layout model** focused on vertical flow and "breathing room."

- **Grid:** A 4-column fluid mobile grid with 20px (`safe-margin`) side margins.
- **Vertical Rhythm:** Elements are grouped using a tight 8px internal spacing, while distinct sections are separated by 40px (`space-xl`) to clearly define focus areas.
- **Mobile-First:** Prioritizes bottom-heavy reachability for all interactive elements, specifically the AI Companion FAB.

## Elevation & Depth
Depth is signaled through **Tonal Layers** and **Soft Ambient Shadows**. 

1. **Base:** The Cream background (#FDFBF7).
2. **Level 1 (Cards):** Pure white (#FFFFFF) with a very soft, diffused shadow: `0px 4px 20px rgba(26, 26, 26, 0.04)`.
3. **Level 2 (Active/Floating):** The same shadow as Level 1 but with an additional `0px 8px 30px rgba(26, 26, 26, 0.08)` to indicate higher prominence, used for the AI Companion and active task modals.
4. **Interactions:** When pressed, cards should "sink" (shadow disappears or reduces) to provide tactile feedback.

## Shapes
The shape language is **exaggeratedly rounded** to remove any visual "sharpness" that could contribute to stress.

- **Standard Cards:** 24px corner radius.
- **Buttons & Chips:** Fully pill-shaped (100px+ radius).
- **Selection States:** Use thick 3px inner-borders in the primary accent color when a card is selected, maintaining the 24px radius.

## Components

- **Task Cards:** Large white containers (24px radius). Include an "Energy State Indicator" (a small soft-colored dot or subtle progress ring) in the top right.
- **Priority Cards:**
  - *"The Thing":* Features a subtle Sage border and slightly larger title.
  - *"Would Be Nice":* Standard card styling.
  - *"Bonus":* Lower opacity (0.7) or a dashed border to denote non-essential status.
- **Energy Selector Chips:** Pill-shaped buttons that use monochromatic shades of the Primary Sage to indicate intensity (Light, Medium, High Energy).
- **AI Suggestion Bubbles:** Asymmetric rounded containers (32px radius with one sharper 8px corner) using a very light tint of Slate Blue (#457B9D) at 10% opacity.
- **Floating Action Button (FAB):** A large, pill-shaped or circular button for the AI Companion, anchored to the bottom-right. It should use the Primary Sage color with high elevation.
- **Input Fields:** Minimalist design with only a bottom-border (2px) in a light neutral, which transforms into the Primary Sage when focused.