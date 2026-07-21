# The UI/UX Design Master Guide
### A complete reference for designing premium, eye-catching, high-quality interfaces (and for redesigning tired ones into something great)

> **Who this is for:** any designer, developer, or AI agent that needs to produce interfaces that look intentional, expensive, and trustworthy — not templated. Read Part 2 first if you only read one thing; it's the thesis the whole guide hangs on.

---

## How to use this guide

This document is organized from *why* → *what* → *how*.

1. **The Laws (Part 1)** explain how human perception and attention actually work. Every visual decision is downstream of these.
2. **The premium thesis (Part 2)** names the specific, checkable things that separate "looks expensive" from "looks like a template."
3. **The craft chapters (Parts 3–13)** give concrete, numeric rules for hierarchy, type, color, space, depth, components, motion, imagery, accessibility, tokens, and copy.
4. **The playbooks (Parts 14–16)** are step-by-step: how to redesign an existing app, what mistakes to avoid, and copy-paste checklists.
5. **The appendix** is drop-in starter values: type scales, spacing scales, shadow recipes, font pairings, and palettes.

**A rule for the whole document:** *specific beats generic.* Every time you're tempted to reach for the default (centered hero, one gradient accent, `border-radius: 8px` on everything, a stock illustration), pause and make a choice that's true to *this* product. Defaults are how work ends up looking AI-generated. Choices are how it ends up looking designed.

---

# Part 1 — The Laws of UX (the psychology layer)

These are the cognitive and perceptual principles that govern how people read, scan, and use interfaces. Each entry: the law, what it means, and the concrete design move it implies.

### Aesthetic–Usability Effect
People perceive attractive interfaces as easier to use, and forgive minor usability flaws when a product looks good. **Move:** polish is not decoration — invest in visual quality early, because it buys trust and patience. But never let beauty *hide* a real usability problem; it only buys forgiveness for small ones.

### Hick's Law
Time to decide grows with the number and complexity of choices. **Move:** reduce options per screen; use progressive disclosure; group and categorize; highlight one recommended path. Onboarding, pricing pages, and settings are the usual offenders.

### Fitts's Law
Time to hit a target depends on its size and distance. **Move:** make primary actions big and close to where the user's attention/cursor already is. Minimum touch target 44×44px (iOS) / 48×48dp (Android). Put destructive actions far from frequent ones. Screen edges and corners are "infinitely large" targets (the cursor stops there) — good for persistent controls.

### Jakob's Law
Users spend most of their time on *other* sites, so they expect yours to work like the ones they already know. **Move:** honor conventions (logo top-left links home, cart top-right, search is a magnifying glass, primary action bottom-right of a dialog). Innovate on value, not on where the nav lives. Break a convention only when the payoff is large and obvious.

### Miller's Law
The average person holds about **7 (±2)** items in working memory. **Move:** chunk information — phone numbers, card numbers, nav groups — into small clusters. Don't use this to justify exactly seven menu items; use it to justify *grouping*.

### Working Memory & Cognitive Load
Working memory is tiny and fragile; every element on screen taxes it. **Cognitive load** splits into *intrinsic* (inherent task difficulty), *extraneous* (caused by bad design), and *germane* (useful effort of learning). **Move:** ruthlessly cut extraneous load — remove redundant fields, unclear labels, unnecessary steps. Carry context forward so users never have to remember what they typed two screens ago.

### Cognitive Bias
Users' judgments are systematically skewed by mental shortcuts (anchoring, framing, loss aversion, etc.). **Move:** design honestly with these in mind — e.g., anchor pricing with a "most popular" tier, but never manipulate (no fake scarcity, no confirmshaming). Dark patterns erode trust and increasingly break the law.

### Chunking
Grouping content into meaningful units makes it easier to process and remember. **Move:** break long forms into steps, split text with headings, group related controls, format long strings (SSNs, IBANs) into readable chunks.

### Law of Proximity
Objects near each other are perceived as a group. **Move:** spacing *is* grouping. A label belongs closer to its own field than to the field above. If two things relate, tighten the gap; if they don't, open it. Proximity is more powerful than borders or boxes for showing relationships.

### Law of Similarity
Elements that share visual traits (color, shape, size) are perceived as related. **Move:** all links look like links; all primary buttons look identical; all destructive actions share a color. Consistency of appearance = communication of function.

### Law of Common Region
Elements inside a shared boundary are perceived as a group. **Move:** cards, panels, and background fills create groups even when items are far apart. Use a subtle background or border to bind a cluster — but proximity is cheaper and often enough.

### Law of Uniform Connectedness
Elements visually connected (by lines, arrows, or a shared container) are perceived as more related than elements merely near each other. **Move:** use connectors for flows, steppers, and relationships; use enclosure for the strongest grouping signal.

### Law of Prägnanz (Simplicity)
People perceive ambiguous or complex images in the simplest form possible. **Move:** favor simple, regular shapes; reduce visual noise; let the eye rest. Simplicity reads as competence.

### Law of Common Fate
Elements moving in the same direction are perceived as related. **Move:** in motion, animate a group together (staggered but coherent) so users read them as one unit; move unrelated things differently.

### Serial Position Effect
People best remember the **first** (primacy) and **last** (recency) items in a series. **Move:** put the most important nav items / actions at the start and end. Bury the least important in the middle.

### Von Restorff Effect (Isolation Effect)
The item that differs most is the one remembered. **Move:** make the single most important action visually distinct — one primary button, one accent color. If everything is emphasized, nothing is. This is *why* you spend your boldness in exactly one place.

### Peak–End Rule
People judge an experience mostly by its most intense moment and its end, not the average. **Move:** invest in the emotional peaks (a delightful success state, a great "aha" moment) and the endings (order confirmation, task completion, sign-off). A great final screen redeems a mediocre middle.

### Goal-Gradient Effect
Motivation increases as people get closer to a goal. **Move:** show progress (progress bars, checklists, "2 of 3 done"). Give visible early progress (pre-filled first step, "endowed progress") to pull people in.

### Zeigarnik Effect
People remember uncompleted tasks better than completed ones. **Move:** surface incomplete states — profile completion meters, unfinished drafts, "3 steps left." Use gently; nagging backfires.

### Doherty Threshold
Productivity soars when system response is under **400ms** — fast enough that user and machine feel in sync. **Move:** respond in <400ms wherever possible. When you can't, use skeleton screens, optimistic UI, and progress feedback so it *feels* fast. Perceived performance often matters more than actual.

### Flow
The mental state of full, energized focus. **Move:** minimize interruptions, keep difficulty matched to skill, give immediate feedback, and remove friction between intent and action. Modals, unexpected dialogs, and layout shifts break flow.

### Selective Attention
People filter out anything irrelevant to their current goal — including things they've learned to ignore (banner blindness). **Move:** don't make important controls look like ads. Place key info in the natural scan path, not the periphery.

### Choice Overload (Paradox of Choice)
Too many options cause anxiety and inaction. **Move:** curate. Offer sensible defaults, "recommended" flags, and filters. Fewer, better options convert more than many mediocre ones.

### Paradox of the Active User
Users start doing things immediately and won't read manuals, even when reading would save time. **Move:** design for people who skip instructions. Make the interface teach itself through affordances, inline hints, and forgiving defaults.

### Tesler's Law (Conservation of Complexity)
Every system has an irreducible amount of complexity; the only question is who absorbs it — the user or the designer/engineer. **Move:** absorb complexity into the product (smart defaults, auto-detection, pre-filled fields) rather than pushing it onto the user.

### Postel's Law (Robustness Principle)
Be liberal in what you accept, conservative in what you produce. **Move:** accept input in any reasonable format (phone numbers with or without spaces/dashes, dates in multiple forms), then normalize it. Forgiving inputs feel premium; rigid validation feels cheap.

### Occam's Razor
Among competing solutions, the one with the fewest assumptions/elements tends to be best. **Move:** remove until it breaks, then add back one. The best design isn't when there's nothing left to add, but nothing left to remove.

### Pareto Principle (80/20)
Roughly 80% of effects come from 20% of causes. **Move:** find the 20% of features/screens users touch most and make those exceptional. Don't spread polish evenly.

### Parkinson's Law
Work (and user effort) expands to fill the time available. **Move:** set expectations and reduce the *perceived* time a task takes — pre-fill, autocomplete, streamline — so people finish faster than they feared.

### Mental Model
Users have an internal belief about how a system works, borrowed from prior experience. **Move:** match your interface to existing mental models; when you must diverge, teach the new model gently. Friction happens where your model and theirs disagree.

**Meta-takeaway of Part 1:** good UI reduces the amount of *thinking* required to act. Every law above is a lever for lowering cognitive cost while raising confidence.

---

# Part 2 — What actually makes a UI look "premium" (the thesis)

Almost nobody can articulate *why* one interface looks like a $200/mo product and another looks like a weekend template — but the difference is concrete and learnable. Premium is not a style. It's the accumulation of the following signals. Master these and any aesthetic (minimal, brutalist, playful, editorial) can read as high-end.

1. **Generous, intentional whitespace.** Cheap UIs cram; premium UIs breathe. Space is not "empty" — it's the frame that makes content feel valuable. When in doubt, add space, especially around headings and between sections.

2. **One accent color, used sparingly.** Amateur work rainbow-vomits color. Premium work is 90% neutral + 10% a single confident accent reserved for the one thing that matters most on each screen (Von Restorff). Scarcity is what makes the accent read as "important."

3. **A real, restrained type system.** Not 5 fonts and 12 sizes — 1–2 families, a deliberate scale, and disciplined weight usage. Tight tracking on large display type. Comfortable line height and measure on body. Type is the single biggest lever for perceived quality.

4. **Consistent spacing rhythm.** Everything sits on a grid (usually 4px/8px). When gaps are arbitrary (13px here, 17px there), the eye can't tell but the brain registers "off." Consistency is invisible; inconsistency is felt as cheapness.

5. **Consistent corner radius and border treatment.** Pick a radius system and apply it religiously. Mixed radii (some 4px, some 12px, some sharp) read as sloppy. Same for borders: one hairline weight, one border color.

6. **Subtle, physically-plausible depth.** Premium shadows are soft, low-opacity, and layered — light comes from one consistent direction. Cheap shadows are harsh, dark, uniform, and everywhere. Often the most premium look uses *almost no* shadow — just hairline borders and background contrast.

7. **Optical alignment and precision.** Things line up. Icons are optically centered (not just mathematically). Baselines align. Nothing is 1px off. This precision is subconscious but decisive.

8. **Real content, real copy.** "Lorem ipsum," "Your Name Here," and stocky placeholder text kill credibility instantly. Design with true content, realistic data, real names, plausible numbers. Copy that sounds human and specific makes a design feel finished.

9. **Purposeful, restrained motion.** Premium motion is quick, eased, and meaningful (it shows cause/effect and spatial continuity). Cheap motion is slow, bouncy everywhere, or absent where it's needed. Less is usually more.

10. **Considered states.** Empty states, loading states, error states, hover/focus/active/disabled, zero/one/many data. Amateur work designs only the happy path. Premium work designs the whole matrix — and treats empty and error states as opportunities, not afterthoughts.

11. **Contrast discipline.** Strong contrast where hierarchy demands it (headline vs. background), soft contrast where it doesn't (secondary text, dividers). Muddy mid-gray text on white everywhere = flat and cheap. A clear light/dark rhythm = crisp and premium.

12. **Details that reward attention.** A perfectly kerned wordmark, a satisfying toggle animation, a thoughtful cursor change, an icon that matches the type weight. Nobody notices any single one; everyone feels their sum.

**The test:** screenshot your UI, blur it heavily. You should still see a clear hierarchy — one focal point, obvious grouping, a calm neutral field with one point of color. If the blur looks like uniform noise, you have no hierarchy yet.

---

# Part 3 — Visual hierarchy (guiding the eye)

Hierarchy is the deliberate ranking of elements so the eye knows where to go first, second, third. It's the backbone of every good layout.

**The tools of hierarchy, roughly in order of power:**

- **Size** — bigger = more important. The strongest, bluntest signal.
- **Weight** — bolder = more important. Great for creating hierarchy without changing size.
- **Color & contrast** — high-contrast/accent pulls the eye; low-contrast recedes. Reserve your highest contrast and your accent for the top of the hierarchy.
- **Space** — isolation creates emphasis; a lone element in whitespace dominates. Surround the hero element with room.
- **Position** — top and left (in LTR reading cultures) get attention first; the natural scan path is a **Z** (for sparse pages) or an **F** (for text-dense pages). Put priorities along that path.
- **Density & style** — italics, uppercase, a different family, a badge, or a rule can all promote an element.

**Rules of thumb:**
- Establish **exactly one** primary focal point per screen. Then a clear secondary, then everything else quiet.
- Limit type roles to about **3 levels** in most views: display/title, body, and caption/meta. More than ~4 and hierarchy turns to mush.
- Don't stack two competing emphases (huge + bold + colored + boxed) on the same element unless it's *the* one thing. Emphasis is a budget; spend it once.
- Use the "squint test": squint at the screen; the elements that still stand out are your hierarchy. If the wrong things stand out, adjust.

---

# Part 4 — Typography (the biggest lever for premium feel)

If you improve only one thing, improve typography. It carries the personality of the whole interface.

## 4.1 Choosing typefaces
- **Fewer families, more weights.** One well-chosen family with a range of weights beats three mismatched families. If pairing, pick **two**: a characterful *display* face (used with restraint for headings) and a highly legible *body* face. Optionally a *mono/utility* face for code, data, or captions.
- **Pair by contrast, harmonize by proportion.** Good pairs contrast in category (serif + sans, or grotesque + humanist) but share compatible x-height and proportions. Safe, high-quality pairings are in the appendix.
- **Avoid the obvious defaults** where you can. System stacks are fine for utility, but a distinctive body/display face is one of the cheapest ways to not look templated. Steer clear of the overused "warm serif display + one clay accent" combo unless the brief calls for it — it now reads as AI-default.
- **Legibility first for body.** Body type must be comfortable at small sizes: generous x-height, open apertures, distinct letterforms (a good `1 l I` and `0 O`).

## 4.2 The type scale
Use a **modular scale** — each step multiplies the last by a ratio, so sizes relate harmonically instead of arbitrarily. Common ratios: 1.125 (subtle), 1.2, 1.25 (very common for UI), 1.333, 1.5 (dramatic, editorial).

A practical UI scale (base 16px, ratio ~1.25), in px:
`12 · 14 · 16 · 20 · 24 · 30 · 36 · 48 · 60 · 72`
Map these to roles rather than using ad-hoc numbers. Fewer distinct sizes = more coherent.

## 4.3 The details that separate pro from amateur
- **Line height (leading):** body text **1.5–1.65**; large headings tighter, **1.05–1.25**. Longer lines need more leading; short lines need less. Tight leading on body = cramped; loose leading on headings = disconnected.
- **Line length (measure):** aim for **45–75 characters** per line, ~66 ideal. Too long tires the eye; too short breaks rhythm. Constrain body containers (e.g. `max-width: 60–75ch`).
- **Letter spacing (tracking):**
  - Large display type: tighten, roughly **-0.01em to -0.04em** (big type looks loose by default).
  - Body: leave at 0 (the designer already tuned it).
  - All-caps / small labels: loosen, roughly **+0.02em to +0.1em** (caps look cramped by default).
- **Weight for hierarchy:** prefer changing weight over changing color to show emphasis. Body 400–450; emphasis 500–600; headings 600–800. Avoid ultra-thin weights for body text (fails on low-quality screens and low contrast).
- **Alignment:** left-align body text (in LTR) — justified web text creates ugly rivers; centered body over more than 2–3 lines is hard to read. Center only short things (a hero headline, a single label).
- **Numbers:** use **tabular (monospaced) figures** for tables, prices, and anything that changes/aligns; use proportional figures for running text.
- **Hyphenation & widows:** avoid single-word last lines (widows) in headlines; use `text-wrap: balance` for headings and `text-wrap: pretty` for body where supported.
- **Case:** sentence case reads faster and feels more modern than Title Case or ALL CAPS for most UI text. Reserve caps for tiny labels/eyebrows (with added tracking).
- **Hierarchy through one family:** you can build a full, elegant hierarchy from a single well-drawn family purely with size, weight, color, and spacing — often the most cohesive choice.

## 4.4 Vertical rhythm
Align type and spacing to a baseline grid (multiples of your base unit, e.g. 4px). Consistent spacing above/below headings creates a calm, professional rhythm. A common pattern: more space *above* a heading than below it, so the heading binds to the content it introduces (proximity).


---

# Part 5 — Color (theory, selection, and building a palette)

Color is where most amateur UIs fall apart and where a disciplined system reads instantly as premium. The goal is **restraint plus a coherent system**, not "more colors."

## 5.1 The mental model: HSL, not hex
Think in **HSL** (Hue, Saturation, Lightness) — it's how designers reason about color, and it makes systematic palettes trivial:
- **Hue (0–360°):** the color itself (red, blue, etc.).
- **Saturation (0–100%):** intensity. High = vivid; low = muted/sophisticated.
- **Lightness (0–100%):** how light/dark.

To build a family of one color (a "tint/shade ramp"), hold hue roughly steady and vary lightness (and slightly nudge saturation). Modern approaches use **OKLCH/LCH** for *perceptually* even steps — steps look equally spaced to the eye, which HSL doesn't guarantee. If your tools support OKLCH, prefer it for generating ramps.

## 5.2 The anatomy of a UI palette
A complete, professional palette has **four groups**:

1. **Neutrals (the workhorse — 90% of the UI).** A ramp of grays from near-white to near-black, usually 9–11 steps (e.g. 50, 100, 200 … 900, 950). These carry text, backgrounds, borders, and surfaces. **Pro move:** give neutrals a subtle temperature by mixing a tiny amount of your brand hue into them (slightly warm or cool gray) instead of pure gray — it makes the whole UI feel considered and cohesive. Pure `#808080` gray is a tell of no color system.

2. **Primary / brand (the accent — ~10%).** One hue, expressed as a full ramp (light tints → base → dark shades). The mid step (~500/600) is your main action color; lighter steps are hover/tints/backgrounds; darker steps are pressed states/text-on-light.

3. **Semantic colors (functional).** Consistent meaning across the product:
   - **Success** = green family
   - **Warning** = amber/yellow family
   - **Danger/Error/Destructive** = red family
   - **Info** = blue family (or your primary if it's blue)
   Each should have a ramp too (a light background tint + a strong foreground/border) so you can build alerts and badges.

4. **Accent / secondary (optional).** A second brand hue for charts, highlights, or illustration. Introduce only if you need it; every extra color is complexity.

## 5.3 Building the ramp (practical recipe)
For each color, generate ~10 steps:
- **50–100:** very light tints — page/section backgrounds, hover fills, badge backgrounds.
- **200–300:** borders, dividers, disabled fills.
- **400:** placeholder text, icons on light, muted accents.
- **500–600:** the "true" color — primary buttons, links, active states. This is the one you name the color after.
- **700–800:** hover/pressed for buttons, strong text, accent-on-light for readability.
- **900–950:** darkest — high-emphasis text, dark backgrounds.

Keep hue nearly constant across the ramp; slightly *increase* saturation toward the mid and *decrease* it at the extremes so lights don't look washed and darks don't look muddy.

## 5.4 Color relationships (choosing which hues)
Classic color-wheel schemes, useful for picking hues that work together:
- **Monochromatic:** one hue, many lightness/saturation levels. Safe, elegant, cohesive. Great default for a restrained brand.
- **Analogous:** neighbors on the wheel (e.g. blue + teal + green). Harmonious, low tension.
- **Complementary:** opposites (blue/orange, purple/yellow). High contrast, energetic — use one as dominant, the other as small accent, never 50/50.
- **Split-complementary / triadic / tetradic:** more colors, more balance needed. Powerful for data viz and illustration; risky for core UI. If you use them, desaturate and let neutrals dominate.

**For most product UIs: neutrals + one primary (mono or analogous) + semantics.** That's it. Restraint reads as premium.

## 5.5 The 60-30-10 rule
A reliable proportion for any composition: **60%** dominant (usually neutral background/surfaces), **30%** secondary (supporting neutrals, secondary surfaces, body text), **10%** accent (your brand color / primary actions). If your accent is creeping past ~10% of the screen, it stops being an accent and hierarchy collapses.

## 5.6 Contrast & accessibility (non-negotiable)
- **Body text:** at least **4.5:1** contrast against its background (WCAG AA). **7:1** for AAA.
- **Large text (≥24px, or ≥18.66px bold):** at least **3:1**.
- **UI components & graphical objects** (icons, input borders, focus rings, chart elements): at least **3:1** against adjacent colors.
- Don't rely on color alone to convey meaning (colorblind users) — pair color with an icon, label, or shape. Error red should also say what's wrong; success green should also show a check.
- Test in grayscale: if hierarchy survives desaturation, it doesn't depend on color perception.

**Pro tip:** the classic mistake is low-contrast light-gray text ("#BBB on #FFF") for "elegance." It fails accessibility *and* reads as unfinished. Use a legitimately readable secondary gray (roughly 4.5:1) instead.

## 5.7 Dark mode
Dark mode is not "invert everything."
- **Don't use pure black (#000) backgrounds.** Use a very dark neutral (e.g. ~#0B0D10 to #16181D). Pure black causes harsh halation against light text and looks like an OLED test screen. Elevated surfaces get *lighter*, not darker (light comes from above → higher surfaces catch more light).
- **Don't use pure white text.** Use ~#E6E8EB / high-90s lightness to reduce glare.
- **Desaturate and lighten your accent** for dark backgrounds — saturated colors vibrate on dark. Lift lightness, drop saturation a touch.
- **Depth via lighter surfaces + subtle borders**, not heavy shadows (shadows barely show on dark). Layer surfaces by lightness: bg < card < popover < modal.
- **Re-check every contrast pair** — dark mode has its own ratios. It's a full second theme, not a filter.

## 5.8 Gradients & color effects (use with taste)
- Subtle gradients (two close hues/lightnesses, a few degrees of hue shift) add life to buttons, hero backgrounds, and accents. Loud rainbow gradients read as dated/cheap.
- Keep gradient contrast low and direction consistent. A mesh/ambient gradient behind a hero can feel premium; a gradient on *every* card does not.
- Avoid the AI-default "one dark background + one acid/vermilion accent" unless the brief truly wants it.

## 5.9 Color, culture, and emotion
Color carries meaning that varies by context and culture (red = danger/luck depending on region; white = purity/mourning depending on culture). For emotional tone: blues read trustworthy/calm/corporate; greens natural/healthy/financial; purples creative/premium; oranges energetic/friendly; warm neutrals cozy; cool neutrals clinical/techy. Choose the hue whose connotations match the product's promise — then let neutrals do the heavy lifting.

---

# Part 6 — Spacing, grid & layout

Spacing is the silent grammar of an interface. Consistent spacing is one of the strongest premium signals; arbitrary spacing is one of the strongest cheap signals.

## 6.1 The spacing scale (8-point grid)
Base everything on a unit — usually **8px**, with 4px as a half-step for fine adjustments. Use a fixed scale, not arbitrary numbers:
`0 · 2 · 4 · 8 · 12 · 16 · 24 · 32 · 40 · 48 · 64 · 80 · 96 · 128`
Every margin, padding, and gap should come from this scale. This creates rhythm and makes the whole UI feel machined rather than hand-wobbled. Component-internal spacing can use 4px steps; layout-level spacing sticks to 8px multiples.

## 6.2 The rule of proximity in layout
Space communicates relationships (Law of Proximity):
- Related items: small gap. Unrelated groups: large gap.
- A label hugs its input; the next field group sits further away.
- Section separation should be clearly larger than in-section spacing (e.g. 64px between sections vs 16px within).
- **When unsure, increase spacing.** Cramped is the more common failure than airy.

## 6.3 Grids & structure
- Use a **column grid** (commonly 12 columns on desktop — divisible by 2, 3, 4, 6) with consistent **gutters** and outer **margins**.
- Define a **max content width** (~1100–1280px for most apps; ~640–720px for reading-focused text) so content doesn't sprawl on wide screens.
- Align elements to the grid; consistent left edges create clean vertical lines the eye follows.
- **Whitespace is structural, not leftover.** Macro whitespace (between big regions) sets the calm; micro whitespace (between lines, around icons) sets legibility. Premium designs are generous with both.

## 6.4 Responsive layout
- **Mobile-first:** design the smallest screen first; it forces prioritization. Then progressively enhance.
- Use fluid units and clamped type (`clamp()`), flexible grids (CSS Grid/Flexbox), and sensible **breakpoints** (common: ~640, 768, 1024, 1280px — but set them where *your content* breaks, not by device).
- Reflow, don't shrink: stack columns, collapse nav into a menu, increase touch targets on mobile (min 44–48px).
- Watch for **layout shift** (CLS): reserve space for images/async content so things don't jump.
- Test the real range: 320px narrow phones up to large desktops, plus zoom to 200%.

## 6.5 Alignment & optical adjustments
- Everything aligns to *something* — a grid line, an edge, a baseline. Random placement reads as amateur.
- **Optical > mathematical:** center icons and glyphs by eye (a play triangle centered mathematically looks off-center), nudge type to sit on the baseline, balance asymmetric shapes. Pixel-perfect math sometimes looks wrong; trust the eye.

---

# Part 7 — Depth, elevation, shadows, borders & radius

Depth done well is a huge premium signal; done badly it's the fastest way to look cheap.

## 7.1 Elevation model
Decide a light source (conventionally top, slightly front) and keep it consistent everywhere. Higher elements cast softer, larger shadows and (in dark mode) sit on lighter surfaces. Build an **elevation scale**: e.g. flat (0) → card (1) → dropdown (2) → modal (3) → toast/popover (4), each with a defined shadow recipe.

## 7.2 Premium shadows (the recipe)
- **Soft and low-opacity.** Real shadows are diffuse. Use large blur, small-to-moderate spread, and **low alpha** (roughly 0.04–0.16), not solid gray.
- **Layer two or three shadows** for realism: one tight/dark-ish shadow close to the element (contact shadow) + one large/soft shadow (ambient). Example concept:
  `0 1px 2px rgba(0,0,0,.06), 0 4px 12px rgba(0,0,0,.08), 0 16px 32px rgba(0,0,0,.06)`
- **Tint the shadow** toward the background/brand hue instead of pure black for a richer feel (e.g. a slightly blue-black shadow on a cool UI).
- **Consistent direction & offset.** All shadows drop the same way. Mixed directions look broken.
- **Less is premium.** Many top-tier UIs use almost no shadow — just a **hairline border + a slightly different surface color** to separate layers. Flat-but-precise often beats heavy-drop-shadow.

## 7.3 Borders & dividers
- One **hairline** weight (1px, or 0.5px on hi-dpi) and one border color (a light neutral, ~200 step). Consistency matters more than the exact value.
- Prefer subtle borders/background contrast over heavy lines. Full-width dividers are often replaceable by whitespace alone (proximity).
- On dark mode, borders are often more important than shadows for separating surfaces.

## 7.4 Corner radius
- Pick a **radius scale** and apply consistently: e.g. `0 (sharp) · 4 (subtle) · 8 (default) · 12 · 16 · 24 · 9999 (pill/circular)`.
- **Nested radius rule:** an inner element's radius should be *smaller* than its container's (inner ≈ outer − padding) so corners stay concentric — a subtle detail that reads as very polished.
- Keep radius consistent by role: all cards share one radius, all buttons another (or the same). Random mixed radii = sloppy.
- Radius sets personality: 0 = serious/editorial/technical; small = neutral/professional; large/pill = friendly/consumer/playful. Choose intentionally per brief.


---

# Part 8 — Components (the building blocks, done right)

For every component, design the **full state matrix**: default, hover, focus (visible!), active/pressed, disabled, loading, error, and (for data components) empty / one / many / overflow. Amateur work ships only "default."

## 8.1 Buttons
- **Hierarchy of button types:** *primary* (filled, accent — one per view ideally), *secondary* (outline or tonal), *tertiary/ghost* (text-only), *destructive* (red, filled or outline). Never two competing primaries in one region.
- **Size & touch target:** comfortable padding (e.g. 10–12px vertical, 16–24px horizontal); min 44px tall for touch. Label + icon gap ~8px.
- **States:** hover (slightly darker/lighter, ~8–12% shift), active (darker + slight scale-down ~0.98 or inset), focus (visible ring, ≥3:1 contrast, offset from the button), disabled (reduced opacity/desaturated + `cursor: not-allowed`, and remove hover), loading (spinner + keep width stable to avoid layout shift).
- **Label:** verb-first, says what happens ("Save changes," "Create account"), sentence case, no vague "Submit/OK." Keep the same verb through the flow (button "Publish" → toast "Published").
- **Radius/weight** consistent with the system; icon weight matches text weight.

## 8.2 Forms & inputs
Forms are where UX is won or lost.
- **One column.** Multi-column forms slow completion and confuse tab order.
- **Labels above fields** (fastest to scan, best for mobile, works with long labels). Avoid placeholder-as-label (disappears on input, fails accessibility).
- **Clear affordance:** inputs look editable (border/fill), sufficient height (~40–48px), visible focus state (accent ring).
- **Helpful validation:** validate on blur/submit, not aggressively on every keystroke; show errors inline next to the field, in plain language, saying how to fix it; keep the user's input; mark the field with color **and** icon **and** text. Show success sparingly.
- **Reduce effort (Tesler/Postel):** smart defaults, autofill/autocomplete, input masks, accept messy formats and normalize, only ask for what you truly need, mark *optional* fields rather than required ones if most are required.
- **Group & chunk** long forms into sections or steps with a progress indicator (Goal-Gradient). Show a review step before irreversible actions.
- **Buttons:** primary action clearly dominant; secondary ("Cancel") quieter; destructive separated. Disable-until-valid is debated — prefer enabling and showing errors on submit so users aren't stuck guessing.

## 8.3 Cards
- Consistent internal padding (from the spacing scale), consistent radius, one elevation treatment.
- Clear internal hierarchy: media → title → supporting text → meta/actions.
- Don't over-border AND over-shadow AND over-fill — pick one grouping method (Common Region). A subtle border *or* a soft shadow *or* a tinted surface, not all three.
- Make the whole card clickable if it's a link (with a visible hover state), but keep nested interactive elements accessible.

## 8.4 Navigation
- Follow conventions (Jakob): top bar or left sidebar; logo top-left → home; primary items along the natural scan path (Serial Position: most-used first and last).
- Show **where you are** (active state) and **where you can go**. Persistent, predictable placement.
- Limit top-level items (group with dropdowns/mega-menus if many; Hick's Law). Use clear, user-language labels — never internal jargon.
- Mobile: collapse to a bottom bar (thumb reach) or a menu; keep the top ~3–5 most important actions reachable.
- Breadcrumbs for deep hierarchies; search for large content sets.

## 8.5 Modals & overlays
- Use sparingly — they interrupt flow. Reserve for focused tasks or confirmations that need full attention.
- Dim the background (scrim), trap focus, close on Esc and scrim click (unless data loss risk), return focus on close.
- Primary action bottom-right (Jakob), destructive confirmations clearly labeled ("Delete 3 files" not "OK").
- Prefer inline expansion, drawers, or dedicated pages for complex tasks; a modal-inside-a-modal is a smell.

## 8.6 Tables & data
- Right-align numbers, use tabular figures, left-align text; keep column headers sticky for long tables.
- Adequate row height and horizontal padding; zebra striping *or* row borders (not both) if it aids scanning.
- Provide sort, filter, and pagination/virtualization for large sets; show total counts.
- Make the primary column scannable (bold/first); de-emphasize meta columns.
- Responsive: convert to stacked cards or allow horizontal scroll with a pinned first column on mobile.

## 8.7 Feedback: toasts, alerts, badges, tooltips
- **Toasts:** transient confirmations, auto-dismiss (~4–6s), non-blocking, one at a time or stacked, with undo where relevant. Never put critical errors only in a toast.
- **Alerts/banners:** semantic color + icon + clear text + (optional) action. Inline near the relevant content.
- **Badges/chips:** small, consistent, semantic; use a light tint background + strong foreground of the same hue.
- **Tooltips:** for supplementary hints only (never essential info), appear on hover/focus, keyboard-accessible, don't obscure the trigger.

## 8.8 Loading & empty states
- **Skeletons > spinners** for content-heavy loads (they suggest structure and feel faster; Doherty). Use spinners for short, indeterminate waits.
- **Optimistic UI:** update immediately, reconcile with the server, roll back on failure — feels instant.
- **Empty states are onboarding:** explain what goes here, why it's empty, and give one clear action to fill it ("Create your first project"). An empty screen is an invitation to act, never a dead end.

## 8.9 Iconography
- One icon set, one style (outline *or* filled, consistent stroke width), one grid size. Mixing sets is a strong cheap-tell.
- Match icon stroke weight to your type weight and size icons to sit optically with text (usually cap-height to a touch larger).
- Icons rarely stand alone — pair with a label unless the icon is universally understood (search, close, menu). Give icon-only buttons `aria-label`s.

---

# Part 9 — Motion & micro-interactions

Motion, done right, explains cause and effect, maintains spatial continuity, and adds delight. Done wrong, it's slow, distracting, and screams "template with a motion library bolted on."

## 9.1 Timing
- **Micro-interactions** (hover, toggle, small state changes): **100–200ms**.
- **Standard transitions** (dropdowns, expanding panels, cards): **200–300ms**.
- **Larger/entering elements** (modals, page/section transitions): **300–500ms**.
- Elements *entering* can be a touch slower; elements *exiting* should be faster (~half) so the UI feels responsive, not laggy.
- Nothing important should wait on animation — never block input behind a slow transition. Respect the Doherty threshold.

## 9.2 Easing
- **Ease-out** (fast start, slow settle) for elements *entering* / responding to user action — feels snappy and natural. This is your default. e.g. `cubic-bezier(0.16, 1, 0.3, 1)` or `cubic-bezier(0, 0, 0.2, 1)`.
- **Ease-in** (slow start, fast end) for elements *leaving*.
- **Ease-in-out** for movement across the screen (both endpoints at rest).
- Avoid pure `linear` except for continuous things (spinners, marquees, progress).
- Springs (with subtle overshoot) feel lively for playful brands; keep overshoot small — bouncy-everywhere reads as juvenile.

## 9.3 What to animate
- Animate **transform** and **opacity** (GPU-cheap, smooth). Avoid animating layout properties (width/height/top/left) where possible — they're janky and cause reflow.
- **Meaningful motion only:** show where a thing came from and went (a modal grows from the button that opened it; a deleted row collapses; a new item slides in). Motion should reinforce spatial/mental models (Law of Common Fate for grouped movement).
- **Micro-interactions of delight:** button press feedback, toggle slide, checkbox draw-in, subtle hover lift, input focus glow, success checkmark. Small, quick, purposeful.
- **Orchestration beats scatter:** one well-timed page-load sequence or scroll reveal lands harder than random effects everywhere. Stagger related items subtly (~30–60ms apart), not chaotically.

## 9.4 Restraint & accessibility
- **Respect `prefers-reduced-motion`** — provide reduced/no-motion alternatives (fade instead of slide, instant instead of animated). This is required, not optional.
- Don't animate on every scroll; don't loop attention-grabbing motion near text; avoid large parallax that induces motion sickness.
- If in doubt, cut it. Excess animation is a leading tell that a design is AI-generated. Motion should feel inevitable, not decorative.

---

# Part 10 — Imagery, illustration & texture

- **Consistency of style** across all imagery (same photographic treatment, or one illustration style, or one 3D style) — mixing styles is a strong cheap-tell.
- **Quality & relevance:** real, high-resolution, on-brand images beat generic stock. If using stock, choose non-obvious shots and apply a consistent treatment (duotone, subtle grade, consistent crop) to unify them.
- **Illustration** can carry brand personality in empty states, onboarding, and marketing — but it must match the product's tone and the rest of the type/color system.
- **Aspect ratios & crops:** consistent ratios across a gallery/grid; use `object-fit: cover` with defined containers to avoid distortion; provide focal points for responsive crops.
- **Performance:** modern formats (AVIF/WebP), responsive `srcset`, lazy-load below the fold, always reserve dimensions to prevent layout shift, always meaningful `alt` text.
- **Texture/noise/grain** used subtly (very low opacity) can add richness and defeat flat sterility — but a heavy overlay everywhere looks dated.
- **Data visualization:** use a restrained, accessible categorical palette (distinct in hue *and* lightness, colorblind-safe); label directly rather than relying on legends where possible; start bar axes at zero; keep chartjunk out.

---

# Part 11 — Accessibility (a11y) — quality, not compliance-theater

Accessible design is better design for everyone; it's also a legal and ethical baseline. Bake it in from the start.

- **Contrast:** meet WCAG AA (4.5:1 text, 3:1 large text and UI/graphics). (See Part 5.6.)
- **Keyboard:** everything operable by keyboard alone; logical tab order; **always-visible focus indicators** (never `outline: none` without a replacement); skip-to-content link; no keyboard traps.
- **Semantics/ARIA:** use native elements (`<button>`, `<a>`, `<label>`, headings in order) before ARIA; add ARIA roles/labels only where needed; label icon-only controls; announce dynamic changes with live regions.
- **Don't rely on color alone;** add icons/text/patterns to convey state.
- **Targets & spacing:** ≥44×44px touch targets; enough spacing to avoid mis-taps.
- **Text:** resizable to 200% without breaking layout; real text over text-in-images; adequate line height and measure (helps everyone, especially dyslexic readers).
- **Motion:** honor `prefers-reduced-motion`; avoid flashing (seizure risk).
- **Forms:** programmatic label↔input association; errors announced and described; instructions not solely visual.
- **Media:** captions, transcripts, and alt text.
- **Test:** with a keyboard only, with a screen reader (VoiceOver/NVDA), at 200% zoom, in grayscale, and with automated tools (axe/Lighthouse) — automated tools catch ~30–40%; manual testing catches the rest.

---

# Part 12 — Design tokens & systems (making it scale + consistent)

Consistency at scale comes from **tokens**: named, reusable design decisions. This is also exactly what makes a codebase produce a coherent premium UI instead of drift.

## 12.1 Token layers
1. **Primitive/global tokens** — raw values: `color-blue-500: #3B82F6`, `space-4: 16px`, `radius-md: 8px`, `font-size-lg: 20px`. No meaning yet.
2. **Semantic/alias tokens** — meaning mapped to primitives: `color-text-primary`, `color-bg-surface`, `color-border-default`, `color-action-primary`, `space-section`, `radius-card`. Components reference *these*, never raw values.
3. **Component tokens** (optional) — component-specific: `button-padding-x`, `card-shadow`.

This indirection is what lets you re-theme (light/dark, brand variants) by swapping the semantic layer, and it prevents one-off values from creeping in.

## 12.2 What to tokenize
Color (with full ramps + semantic roles), spacing scale, type scale (size/line-height/weight/tracking/family), radius scale, border widths, shadow/elevation scale, z-index layers, breakpoints, motion durations & easings, opacity levels, icon sizes. If a value appears more than once, it should be a token.

## 12.3 Naming & governance
- Name by **role**, not appearance (`color-danger`, not `color-red` — so red isn't hard-coded into meaning).
- Keep a single source of truth; document each token's intent and usage.
- Build a component library on top (buttons, inputs, cards, etc.) with documented props and states so every screen composes from the same parts.
- **Consistency is the product.** A design system exists so the 500th screen looks as intentional as the first.

---

# Part 13 — UX writing (words are design material)

Copy makes a design feel finished or fake. Bring the same care to words as to spacing and color.

- **Write from the user's side of the screen.** Name things by what the user controls and recognizes, never by how the system is built ("Notifications," not "Webhook config").
- **Active voice, verb-first controls.** A button says exactly what it does ("Save changes"). Keep the verb consistent across the flow.
- **Sentence case, plain words, no filler.** Specific beats clever. Every element does exactly one job — a label labels, an example demonstrates.
- **Errors:** never vague, never blame the user, never just "apologize." Say what happened and how to fix it, in the interface's calm voice.
- **Empty states:** direction, not mood — tell people what to do next.
- **Tone matched to brand and audience;** conversational but tuned. Microcopy (button labels, helper text, confirmations, tooltips) is where trust is quietly built.
- **No placeholder text in final designs.** Real, plausible content everywhere — it's a top premium/authenticity signal.


---

# Part 14 — The redesign playbook (existing app → awesome)

A step-by-step method for taking a tired interface and making it premium. Diagnosis first; don't reskin blindly.

**Step 1 — Understand before touching.** What is this product's single job? Who uses it, in what context? What are the 20% of screens/flows they use 80% of the time (Pareto)? Redesign *those* first and hardest.

**Step 2 — Audit the current state.** Screenshot the key screens. For each, list: what's the intended focal point vs. the actual one? Where is hierarchy unclear? Count the fonts, sizes, colors, radii, shadow styles, and spacing values in use — the counts alone usually reveal the chaos. Note broken/missing states (empty, error, loading).

**Step 3 — Fix the foundation before the surface.** Establish the systems in this order, because each depends on the last:
   1. **Spacing scale** (adopt an 8px system; snap everything to it).
   2. **Type system** (choose 1–2 families, set a modular scale, define ~3–4 roles, fix line-height/measure/tracking).
   3. **Color system** (neutrals ramp with a subtle temperature + one primary ramp + semantics; enforce 60-30-10 and contrast).
   4. **Radius + border + elevation** (one radius scale, one border weight/color, a soft layered shadow set — or hairline-only).
   5. **Component states** (design the full matrix for buttons/inputs/cards/nav).
   These five systems alone transform most apps.

**Step 4 — Re-establish hierarchy screen by screen.** One focal point each. Cut competing emphasis. Add whitespace (usually the single biggest visible improvement). Apply the squint/blur test.

**Step 5 — Reduce.** Remove redundant elements, merge duplicate patterns, delete decorative noise (Occam). Absorb complexity for the user (Tesler): smarter defaults, fewer required fields, pre-filled data.

**Step 6 — Add craft & delight, sparingly.** Now (and only now) add purposeful micro-interactions, a considered empty/success state (Peak-End), and one signature element the product is remembered by. Spend boldness in one place.

**Step 7 — Fix copy.** Rewrite labels, errors, and empty states per Part 13. Replace all placeholder text with real content.

**Step 8 — Verify.** Accessibility pass (keyboard, contrast, focus, screen reader), responsive pass (320px → wide, 200% zoom), reduced-motion pass, and the state matrix (empty/loading/error/overflow). Then remove one more thing (Chanel's rule).

**Golden rule of redesign:** most "ugly" apps aren't missing decoration — they're missing *systems and hierarchy*. Fix consistency and focus, and 80% of the ugliness disappears before you've added a single flourish.

---

# Part 15 — Anti-patterns & AI-generated "tells" to avoid

If your design has these, it will read as cheap or machine-made. Hunt them down.

**Cheap / amateur tells**
- Arbitrary, off-grid spacing (13px, 17px, 23px…). → Snap to the scale.
- Too many fonts / too many sizes / too many weights. → 1–2 families, ~3–4 sizes.
- Rainbow of accent colors; accent used everywhere. → One accent, ~10%.
- Pure `#000` text/bg, pure `#808080` gray, harsh pure-black shadows. → Off-black, tinted neutrals, soft low-alpha shadows.
- Mixed corner radii and mixed border weights. → One radius scale, one border style.
- Low-contrast light-gray text "for elegance." → Fails a11y and looks unfinished.
- Placeholder/lorem text and stock-photo obviousness. → Real content, curated imagery.
- Only the happy path designed; broken empty/error/loading states. → Full state matrix.
- Cramped layouts with no breathing room. → More whitespace.
- Centered everything / centered long body text. → Left-align body, center only short bits.

**AI-generated tells (avoid these defaults specifically)**
- The warm cream background + high-contrast serif display + single terracotta/clay accent (≈ `#D97757`). It's now the signature "AI made this" look.
- Near-black background + one acid-green or vermilion accent, regardless of subject.
- Generic broadsheet layout: hairline rules, zero radius, dense newspaper columns applied to everything.
- The `01 / 02 / 03` numbered-marker motif used where the content isn't actually a sequence.
- "Big number + small label + gradient accent" hero on every project.
- One gradient on every card; bouncy animation on every element; a signature "glow" everywhere.
- Symmetrical, evenly-weighted layouts with no real focal point (everything equally emphasized = nothing emphasized).

**Dark patterns (never do — unethical and increasingly illegal)**
- Confirmshaming, fake scarcity/countdowns, pre-checked opt-ins, hidden costs, roach-motel cancellation, disguised ads, tricky defaults. They convert short-term and destroy trust (and violate the honest use of cognitive bias from Part 1).

---

# Part 16 — Checklists (copy-paste)

## 16.1 New-screen quick checklist
- [ ] One clear focal point (passes squint/blur test)
- [ ] Hierarchy uses ≤ ~4 type roles; emphasis spent once
- [ ] All spacing from the scale (8px grid); generous whitespace
- [ ] 60-30-10 color balance; one accent; neutrals carry the UI
- [ ] Text contrast ≥ 4.5:1 (≥3:1 large / UI); not color-alone
- [ ] Body measure 45–75ch, line-height ~1.5; headings tight
- [ ] Consistent radius, border weight, and one elevation style
- [ ] Real content and copy (no lorem/placeholder)
- [ ] Verb-first, sentence-case labels; consistent action verbs

## 16.2 Component / state checklist (per interactive element)
- [ ] Default / hover / focus (visible ring) / active / disabled / loading
- [ ] Error + helpful message (for inputs)
- [ ] Empty / one / many / overflow (for data components)
- [ ] Touch target ≥ 44px; keyboard operable; aria-label if icon-only
- [ ] No layout shift between states

## 16.3 Pre-ship checklist
- [ ] Keyboard-only pass (tab order, focus visible, no traps, skip link)
- [ ] Screen-reader pass (labels, headings order, live regions)
- [ ] Contrast + grayscale pass
- [ ] Responsive: 320px → wide, columns reflow, 200% zoom OK
- [ ] `prefers-reduced-motion` respected
- [ ] Images: alt text, dimensions reserved, modern formats, lazy-load
- [ ] Dark mode (if any) is a full, contrast-checked theme
- [ ] Copy reviewed: errors, empty states, microcopy
- [ ] Remove one more thing (Chanel's rule)

## 16.4 The "does it look premium?" gut-check
- [ ] Blurred, it still has a clear focal point and calm neutral field
- [ ] Feels spacious, not cramped
- [ ] Colors feel restrained and intentional (one accent)
- [ ] Type feels crafted (not default system + random sizes)
- [ ] Depth is subtle and consistent (or intentionally flat + precise)
- [ ] Nothing is 1px misaligned; icons optically centered
- [ ] Motion is quick, eased, and purposeful (or absent)
- [ ] It doesn't match any of the AI-default looks in Part 15
- [ ] There's exactly one memorable signature element

---

# Appendix — Drop-in starter values

Tune to the brief; these are sane, professional defaults, not laws.

## A. Spacing scale (px)
`2, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96, 128`
Layout-level: 8px multiples. Component-internal fine-tuning: 4px steps.

## B. Type scale (px, base 16, ratio ~1.25)
`12, 14, 16, 20, 24, 30, 36, 48, 60, 72`
Roles: caption 12–14 · body 16 · lead 20 · h4 24 · h3 30 · h2 36–48 · h1/display 60–72.
Line-height: body 1.5–1.65 · headings 1.05–1.25. Tracking: display -0.02em · caps labels +0.04em.

## C. Radius scale (px)
`0, 4, 8, 12, 16, 24, 9999(pill)` — default UI radius often 8–12. Inner radius < outer radius.

## D. Elevation / shadow recipes (light mode)
- Level 1 (card): `0 1px 2px rgba(16,24,40,.06), 0 1px 3px rgba(16,24,40,.08)`
- Level 2 (dropdown): `0 4px 8px rgba(16,24,40,.06), 0 8px 16px rgba(16,24,40,.08)`
- Level 3 (modal): `0 8px 16px rgba(16,24,40,.08), 0 20px 40px rgba(16,24,40,.10)`
Tint the rgba toward your brand hue for extra polish. In dark mode, prefer lighter surfaces + subtle borders over shadows.

## E. Motion tokens
Durations: `fast 120ms · base 200ms · slow 320ms · xslow 480ms`.
Easing: entrance/standard `cubic-bezier(0.16, 1, 0.3, 1)` · exit `cubic-bezier(0.4, 0, 1, 1)` · move `cubic-bezier(0.65, 0, 0.35, 1)`. Exits ~half the duration of entrances. Animate transform/opacity only.

## F. Neutral ramp with subtle temperature (cool-gray example)
`50 #F8FAFC · 100 #F1F5F9 · 200 #E2E8F0 · 300 #CBD5E1 · 400 #94A3B8 · 500 #64748B · 600 #475569 · 700 #334155 · 800 #1E293B · 900 #0F172A · 950 #020617`
(For a warm variant, nudge hue toward ~30–40° and keep saturation low.)

## G. Semantic starter (light bg tint / strong fg)
- Success `#ECFDF5 / #059669`
- Warning `#FFFBEB / #D97706`
- Danger `#FEF2F2 / #DC2626`
- Info `#EFF6FF / #2563EB`
Build a full ramp per color for production; verify contrast on your real surfaces.

## H. Curated font pairings (deliberate, not the AI-default)
Choose per brief; pair a display face with a legible body face.
- **Editorial / trustworthy:** *Fraunces* or *Newsreader* (display) + *Inter* or *IBM Plex Sans* (body)
- **Modern SaaS / clean:** *Geist* or *Satoshi* (display/body) + *Geist Mono* (data)
- **Warm / human:** *Bricolage Grotesque* (display) + *Source Serif / Literata* (body)
- **Technical / precise:** *Space Grotesk* (display) + *IBM Plex Sans* (body) + *IBM Plex Mono* (data)
- **Elegant / premium:** *Playfair Display* or *Canela* (display) + *Inter* / *Söhne*-like grotesque (body)
- **Friendly / consumer:** *Cabinet Grotesk* (display) + *General Sans* (body)
- **Safe system stack (utility/fallback):** `ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, sans-serif`
Avoid defaulting to a warm serif display + single clay accent — it reads as AI-generated.

## I. Sample palette directions (neutrals + one primary + semantics)
- **Deep indigo tech:** neutrals cool-gray (ramp F) + primary indigo (`#4F46E5` mid) + semantics (G). Trustworthy, techy.
- **Forest / finance:** warm-gray neutrals + primary green (`#15803D` mid) + gold accent. Stable, natural.
- **Editorial ink:** near-black ink neutrals + a single restrained rust or ultramarine accent. Serious, refined.
- **Soft consumer:** warm off-white bg + rounded UI + primary coral or violet (kept ≤10%). Friendly, approachable.
Always: neutrals dominate, one accent at ~10%, contrast verified, dark-mode variant designed separately.

---

## Closing principle

Premium UI is not a style you apply — it's the visible result of **restraint, consistency, hierarchy, and craft**, grounded in how people actually perceive and think (Part 1). Build the systems first, spend boldness in exactly one place, design every state, use real content, remove one more thing than feels comfortable — and let one memorable signature element carry the personality. Do that, and almost any aesthetic direction will read as intentional, trustworthy, and expensive.

---

# Part 17 — Best color combinations (ready-to-use pairings)

Color *theory* is in Part 5; this section is the practical answer to "which colors go together." Every palette below is built the premium way: **neutrals dominate, one accent at ~10%, contrast verified.** Each combo lists background → surface (cards) → primary text → secondary text → border → **accent** (+ a hover shade). Copy, then tune hue to taste.

## 17.1 How to pick a combination that works
Three reliable formulas, easiest to hardest:
1. **Monochrome + one accent (safest).** One neutral ramp (with a subtle temperature) + a single accent hue. Almost impossible to make ugly. Use this if unsure.
2. **Analogous accents.** Accent + a neighbor on the wheel (blue+teal, violet+magenta) for charts/highlights. Low tension, cohesive.
3. **Complementary accent + neutral.** Warm accent on cool neutrals (or vice-versa) — e.g. amber accent on slate-gray UI. High energy, but keep the accent to ~10% and let it fight nothing else.

**Golden rule for pairing an accent to neutrals:** match temperature deliberately.
- **Cool neutrals** (blue-gray) pair cleanly with cool accents (blue, indigo, teal) or *pop* with one warm accent (amber, coral).
- **Warm neutrals** (beige, taupe) pair with warm accents (rust, ochre, olive) or pop with one cool accent (teal, deep blue).
- Pure-gray neutrals go with anything but feel the least "designed."

## 17.2 Proven light-mode palettes (full hex)

**A. Indigo SaaS — clean, trustworthy, techy (great default)**
`bg #F8FAFC · surface #FFFFFF · text #0F172A · text-2 #475569 · border #E2E8F0 · accent #4F46E5 · accent-hover #4338CA`

**B. Ocean blue — calm, corporate, dependable**
`bg #F7F9FB · surface #FFFFFF · text #0B2540 · text-2 #51657A · border #DDE5EC · accent #0EA5E9 · accent-hover #0284C7`

**C. Forest / finance — stable, natural, "money"**
`bg #F6F7F4 · surface #FFFFFF · text #14231A · text-2 #4B5A50 · border #E3E7DE · accent #15803D · accent-hover #166534`
Optional second accent: brass `#B8860B` for highlights.

**D. Emerald + charcoal — fresh, modern, confident**
`bg #FAFAFA · surface #FFFFFF · text #18181B · text-2 #52525B · border #E4E4E7 · accent #059669 · accent-hover #047857`

**E. Violet creative — premium, imaginative, brandy**
`bg #FAF9FC · surface #FFFFFF · text #1E1B2E · text-2 #57516B · border #E9E5F0 · accent #7C3AED · accent-hover #6D28D9`

**F. Coral / consumer — friendly, warm, approachable**
`bg #FFF9F6 · surface #FFFFFF · text #2A1D18 · text-2 #6B5750 · border #F1E4DD · accent #F0603C · accent-hover #DC4B28`
Pairs beautifully with a teal secondary `#0D9488` (complementary).

**G. Amber on slate — energetic pop on a cool, serious base**
`bg #F8FAFC · surface #FFFFFF · text #0F172A · text-2 #475569 · border #E2E8F0 · accent #F59E0B · accent-hover #D97706`

**H. Editorial ink — refined, serious, magazine-like**
`bg #FBFBF9 · surface #FFFFFF · text #1A1A1A · text-2 #565654 · border #E7E7E2 · accent #B4472E (rust) · accent-hover #9A3B26`
Swap the rust for ultramarine `#274690` for a cooler, colder tone.

**I. Rose / warm luxury — soft, elegant, boutique**
`bg #FDF8F6 · surface #FFFFFF · text #2B1A1E · text-2 #6E555B · border #F0E1E1 · accent #BE185D · accent-hover #9D174D`
Add gold `#C6A15B` for a jewelry/premium feel.

**J. Teal wellness — health, calm, clinical-but-warm**
`bg #F5FAF9 · surface #FFFFFF · text #0F2724 · text-2 #4A625E · border #DCEAE7 · accent #0D9488 · accent-hover #0F766E`

## 17.3 Proven dark-mode palettes (full hex)
Remember: never pure black, never pure white, elevated surfaces get *lighter*, accents desaturated/lightened, depth via surfaces + subtle borders.

**K. Midnight indigo (pairs with A)**
`bg #0B0D12 · surface #151822 · surface-2 #1D2130 · text #E6E8EE · text-2 #9AA1B2 · border #262B3A · accent #818CF8 · accent-hover #A5B0FF`

**L. Slate + cyan — classic developer/technical**
`bg #0A0F16 · surface #121A24 · surface-2 #18222F · text #E5EAF0 · text-2 #93A1B3 · border #22303F · accent #22D3EE · accent-hover #67E8F9`

**M. Deep forest — premium, calm dark green**
`bg #0B120E · surface #14201A · surface-2 #1C2C24 · text #E7EDE9 · text-2 #93A69C · border #26382F · accent #34D399 · accent-hover #6EE7B7`

**N. Charcoal + amber — warm, cozy dark**
`bg #0F0E0C · surface #1A1815 · surface-2 #242019 · text #EDE9E3 · text-2 #A69E92 · border #302B23 · accent #FBBF24 · accent-hover #FCD34D`

**O. Violet noir — creative, moody**
`bg #0D0B12 · surface #17141F · surface-2 #201B2C · text #EAE6F0 · text-2 #A297B5 · border #2C2439 · accent #A78BFA · accent-hover #C4B5FD`

## 17.4 Combinations by mood / industry (quick lookup)
- **Trust / finance / B2B:** navy or indigo base + green or gold accent → palettes A, B, C, K.
- **Health / wellness / calm:** teal/green + white + soft warm accent → J, M.
- **Luxury / premium / editorial:** near-black or cream + one jewel or metallic accent (gold, deep rust, burgundy) → H, I, N.
- **Creative / playful / brandy:** violet, coral, or teal+orange complementary → E, F, O.
- **Developer / technical / data:** near-black + one neon (cyan/green/electric-blue) → L, K.
- **Consumer / friendly / lifestyle:** warm off-white + rounded UI + coral/violet → F, I.
- **Energetic / bold:** cool neutral base + one hot complementary accent (amber/coral) at ~10% → G, F.

## 17.5 Accent pairs that harmonize (for charts, dual-accents, gradients)
Use these when you legitimately need two colors (data viz, illustration, a two-stop gradient):
- Indigo `#4F46E5` + Cyan `#06B6D4` (analogous-ish, cool, techy)
- Violet `#7C3AED` + Pink `#EC4899` (analogous, vibrant, creative)
- Teal `#0D9488` + Amber `#F59E0B` (complementary, balanced, lively)
- Blue `#2563EB` + Orange `#F97316` (complementary, classic, high-energy)
- Emerald `#059669` + Lime `#84CC16` (analogous, fresh)
- Rose `#E11D48` + Amber `#F59E0B` (warm analogous, food/hospitality)
Keep one dominant and one supporting — never a 50/50 split in the core UI.

## 17.6 Palettes extracted from real reference designs (color-sampled)
These were sampled directly from strong real-world UIs — accents are the true vivid values, neutrals the true fields. Each is a proven, distinctive combination.

**R1 · Botanical dark** — warm-charcoal e-commerce; product photography carries the color, UI stays quiet.
`bg #161616 · surface #1F1F1F · surface-2 #242423 · text #F2F1EE · text-2 #A8A69E · border rgba(255,255,255,.08) · accent(green) #5FAE5A` — imagery pops: clay `#CF7F64`, gold `#D7B351`.

**R2 · Coral analytics (light)** — warm, energetic dashboard; one dark featured card among white ones.
`bg #F0F0F0 · surface #FFFFFF · featured-surface #1D1D1C · text #1D1D1C · text-2 #6E6B64 · border #E4E1DB · accent #F3673F · accent-2(yellow) #F5D400 · positive #37C978`

**R3 · Lime + orange dark** — striking analytics palette; lime + orange + white on near-black.
`bg #0F0F0F · surface #1A1A1A · surface-2 #1F1F1F · text #F4F4F2 · text-2 #8C8C86 · border rgba(255,255,255,.07) · accent(lime) #B7F580 · accent-2(orange) #E08A2E · data-white #FFFFFF`

**R4 · Mint minimal (light)** — ultra-clean; black + white + a single mint green.
`bg #FFFFFF · alt-surface #F5F5F5 · text #0A0A0A · text-2 #565656 · border #E6E6E6 · accent(fill) #8CE78F · accent(text/contrast) #22C55E`

**R5 · Warm-gray + dual pop** — neutral gray gradient base with two complementary sparks for floating glass cards.
`bg gradient #EDEDED→#C1C2C3 · surface rgba(255,255,255,.6) glass · text #0A0A0A · text-2 #6B6B6E · border rgba(0,0,0,.08) · accent(warm) #E4703F · accent(cool) #4C86C4`

**R6 · Sage teal editorial (light)** — calm, premium, photo-forward.
`bg #F5F5F4 · surface #FFFFFF · text #2A322F · text-2 #5C6763 · border #E4E6E4 · accent(teal) #2E9E94 · warm spark #EA6B82 (rare) · supporting sage #899E9C`

**R7 · Gold noir** — rich, high-end (gaming/luxury); warm-black + gold + white.
`bg #100F0F · surface #1B1A18 · surface-2 #252624 · text #F1EEE8 · text-2 #A39E94 · border rgba(255,255,255,.08) · accent(gold) #E6B24A · gold-ink #C79736`

**What they share (the lesson):** in every one, **near-monochrome neutrals dominate and one or two vivid accents do all the talking.** Even the "colorful" dashboards (R2, R3) are 85%+ neutral. That ratio — not the specific hue — is the premium signal.

## 17.7 Quick pairing cheat-sheet (accent ← base)
- Warm-charcoal / black base → green, gold, lime, orange, or single-photo color (R1, R3, R7).
- Off-white / light-gray base → coral, mint, teal, cobalt (R2, R4, R6, and Part 17.2).
- Neutral true-gray base → dual complementary sparks, one warm + one cool (R5).
- Any base → ruthlessly keep accents to ~10–15% of the pixels.

## 17.8 Combinations to avoid (clashes & cheap-tells)
- **Red + green** at similar saturation/lightness (vibrates; also invisible to red-green colorblind users). If both are needed (success/error), differ them in lightness too and add icons.
- **Blue + purple** as *both* accents — too close, reads as a rendering bug; pick one.
- **Two saturated warm colors** fighting (red + orange + yellow all loud) — pick one, mute the rest.
- **High-saturation color as a large background** with body text on it — tiring and hard to read; keep saturated color to accents/small areas.
- **Neon + neon** (e.g. cyan + magenta both maxed) outside of a deliberate retro/synthwave brief — looks amateur.
- **Pure complementary at 50/50** (equal red and green, equal blue and orange) — no hierarchy, maximum tension.
- **More than one accent hue in standard UI** — the fastest way to look un-designed. One accent, always.

## 17.9 One-minute palette build (repeatable recipe)
1. Pick the **mood** (17.4) → gives you an accent hue.
2. Pick neutral **temperature** to match/contrast the accent (17.1).
3. Generate the **neutral ramp** (50→950) with a trace of the accent hue mixed in.
4. Set **accent** = the ~500/600 step of that hue; **accent-hover** = one step darker.
5. Add **semantics** (green/amber/red/blue) from Appendix G.
6. **Verify contrast** (4.5:1 text, 3:1 UI) on your real surfaces.
7. Build the **dark variant** as a separate theme (17.3 as a starting point).
Result: a complete, coherent, premium palette — neutrals carry it, one accent makes it memorable.

---

# Part 18 — Modern layout systems & composition patterns

Beyond the basic grid (Part 6), these are the layout archetypes behind most contemporary premium products.

## 18.1 Bento grids
Modular tiles of *varying* sizes packed into a grid (like a bento box) — a signature look of current dashboards, feature sections, and marketing pages. Rules that make them work:
- Vary tile size to encode importance (the hero metric spans 2×2; supporting items 1×1). Size = hierarchy.
- Keep consistent gaps and one radius across all tiles; align tiles to a shared grid so edges line up.
- Let each tile do exactly one job (one metric, one chart, one CTA). Don't cram.
- Balance the composition — distribute visual weight so it doesn't lean; use one accent tile among neutral ones (Von Restorff).

## 18.2 Dashboard composition
Top-down reading order for an analytics view: global controls (search, date range, filters) → headline KPIs (a row of stat cards) → primary chart(s) → secondary/detail tables. Group by question the user is answering, not by data source. Keep one column width system so cards align. Reserve one **featured card** (often inverted/darker) for the single most important metric.

## 18.3 Faceted catalog layout
The e-commerce/list archetype: a persistent **filter sidebar** (categories, price, size, tags) + a **results grid** + a **sort/active-filter bar** on top. Keep the sidebar narrow and scannable; show result counts per facet; make active filters visible and removable as chips above the grid (see 20.2). On mobile, collapse filters into a drawer/sheet.

## 18.4 Floating cards over imagery
A hero photo/illustration with small UI cards (a stat, a chat bubble, a product, a rating) layered on top at varied depths. Feels dynamic and "productized." Keep the floating cards few (3–4 max), give them consistent radius + soft shadow, and let them overlap the image edge slightly for depth. Ensure text stays readable (a scrim or the cards' own solid backgrounds).

## 18.5 Asymmetric / split hero
Content on one side, visual/signature on the other (roughly 50/50 or 55/45). More editorial and confident than a centered hero. Anchor the eye with one large element; let whitespace carry the rest. This is usually a stronger, less templated choice than the centered "big headline + subhead + two buttons" default.

## 18.6 Overlapping & editorial cards
Cards that break the grid — overlapping image tiles, cards that bleed off-edge, rotated/tilted elements. Powerful for editorial/travel/portfolio work. Use sparingly and deliberately; overlap should imply relationship or depth, not chaos.

---

# Part 19 — Dashboards & data display

## 19.1 KPI / stat card anatomy
A great metric card, top to bottom: **label** (what this measures, small, muted) → **value** (large, tabular figures, the focal point) → **delta/trend** (change vs. prior period) → optional **context** (sparkline or "vs last month"). Keep all cards in a row the same height and value size so they scan as a set.

## 19.2 Delta & trend indicators
Show change, not just state: an **up/down triangle or arrow + percentage**, colored semantically (green up = good, red/orange down = bad — but flip for metrics where down is good, e.g. churn). Always pair the color with a glyph and a sign (▲ +12% / ▼ −3%) so it survives colorblindness and grayscale. Add the comparison basis ("than last month") in muted text.

## 19.3 The featured-card pattern
Among a row of light/neutral cards, render the single most important one in an inverted (dark or accent-filled) treatment. It instantly becomes the focal point (Von Restorff) without changing size. A reliable, premium dashboard move.

## 19.4 Choosing the right chart
- **Trend over time →** line or area chart.
- **Compare categories →** bar/column chart.
- **Part-to-whole →** donut/stacked bar (avoid pie beyond ~3–4 slices; label directly).
- **Progress toward a goal →** gauge/radial or progress bar.
- **Distribution →** histogram/box.
- **Compact inline trend →** sparkline (no axes, just shape — great inside stat cards).
- **Scheduling/duration →** Gantt/timeline bars.
- **Correlation →** scatter.
Match the chart to the *question*; don't decorate data with the flashiest chart.

## 19.5 Chart styling for premium feel
- Restrained, accessible categorical palette — colors distinct in **hue and lightness**, colorblind-safe; ideally one accent + neutrals rather than a rainbow.
- Kill chartjunk: no heavy gridlines (use faint or dotted baselines), no 3D, no drop shadows on bars. Light dotted average/target lines are useful.
- **Label directly** where possible instead of forcing legend lookups; if a legend is needed, use small colored dots + labels.
- Bars: start the axis at zero, consistent bar width, rounded top corners are fine if used consistently.
- **Tooltips** on hover/focus: show exact value + context, positioned near the point, keyboard-accessible.
- Round numbers for humans ($45.8k, not $45,786.00) except where precision matters.
- Reserve space and animate values/bars in subtly on load (ease-out, staggered) — but don't block reading.

## 19.6 Data states
Design the empty ("No data yet — connect a source"), loading (skeleton chart shapes, not just a spinner), partial, and error states for every data component. A chart with no empty state looks broken the moment data is absent.

---

# Part 20 — E-commerce & catalog patterns

## 20.1 Product card anatomy
**Image** (consistent aspect ratio, `object-fit: cover`, generous padding for products shot on white/transparent) → **favorite/wishlist** toggle (top corner) → **name** → optional short **descriptor** → **price** (tabular figures; show original + sale price when discounted) → optional **rating** and **quick-add**. Keep the whole card an obvious click target with a clear hover state (lift + subtle shadow). Consistency across every card is what reads as premium.

## 20.2 Faceted filtering
- **Filter sidebar** groups: search, categories (with counts), price range, size/variants, tags/attributes.
- **Active filters as removable chips** above the results ("Luxury ✕", "Size: M ✕") + a "Clear all" — this makes current state visible and reversible (a top usability win).
- **Result count** updates live ("128 results"); show counts per facet so users avoid dead-end filters (Postel/Tesler — reduce wasted effort).
- Multi-select facets use checkboxes/toggle chips; single-select uses radios/segmented controls.

## 20.3 Range & histogram sliders
A dual-handle slider for price/size, ideally over a **histogram** of how many items fall in each band (so users see where the inventory is). Show the numeric min/max inputs alongside for precision. Snap to sensible steps.

## 20.4 Variant & size selectors
Size/color as a row of selectable chips or swatches (color = actual swatch, not text). Show unavailable variants as disabled (struck-through), not hidden, so users learn what exists. Selected state must be unmistakable (filled + border + checkmark).

## 20.5 Sort, quick actions, featured items
Sort dropdown ("Default / Price / Newest / Rating"). Quick-add and wishlist on hover. To feature an item, use a distinct treatment — a subtle tilt/rotation, elevation, or an accent border/ribbon — to break the grid rhythm and draw the eye (used judiciously). Featured ≠ every card; scarcity is the point.

---

# Part 21 — Extended component library

Components the core chapters didn't cover, all seen across premium products.

## 21.1 Tabs
Switch between views in the same context. One active tab clearly marked (underline, filled pill, or weight change). Keep labels short, order by importance, keep the content area stable (no jump on switch). Don't use tabs for sequential steps (that's a stepper) or for unrelated destinations (that's nav).

## 21.2 Segmented controls
A compact group of 2–4 mutually exclusive options in one pill (e.g. Week / Month, List / Grid). The selected segment is filled; the track is a subtle inset. Great for view toggles inside cards. Beyond ~4 options, use a dropdown instead (Hick's Law).

## 21.3 Chips & tags
Small pill-shaped elements for filters, categories, and metadata. Three flavors: **selectable** (toggle on/off, filled when active), **removable** (with an ✕, for active filters), and **static** (labels/status). Consistent height, padding, and radius; semantic tint background + matching foreground for status chips.

## 21.4 Avatars & avatar stacks
Circular, consistent size, with an image or initials fallback (colored background + white initials). **Stacks** overlap avatars (negative margin) with a "+3" chip for overflow — a compact way to show membership/assignees. Add a thin ring in the surface color so overlapping avatars separate cleanly.

## 21.5 Count & notification badges
Small numeric or dot badges on icons (unread, cart count). Keep tiny, high-contrast, positioned top-right of the target; cap at "9+"; use a dot (no number) for "something new" when the exact count doesn't matter.

## 21.6 Navigation variants
- **Top bar** — default for marketing + broad apps.
- **Left sidebar** — deep, multi-section apps.
- **Icon rail** — narrow vertical strip of icons (with tooltips/labels on hover) for space-efficient primary nav.
- **Pill / floating nav** — rounded, detached nav bar (often with icon+label pills) — modern, distinctive; keep items few.
- **Bottom bar** — mobile primary nav (thumb reach), 3–5 items.
- **FAB (floating action button)** — one persistent primary action (usually "create/add"), bottom corner; never more than one; not for navigation.

## 21.7 Circular icon buttons
Round buttons holding a single icon (arrow, play, add) — a clean editorial device. Keep a consistent diameter, optically-centered icon, clear hover, and a real `aria-label`. Great for "next/prev," "learn more →," and media controls.

## 21.8 Search field
Magnifying-glass affordance (leading icon), clear placeholder ("Search plants…"), optional filter button trailing, and a clear (✕) affordance when populated. For large sets, show suggestions/recent searches on focus.

## 21.9 Star ratings & reviews
Filled/half/empty stars + the numeric value + review count ("4.5 · 1,024 reviews"). Use one accent (often amber) for stars; keep half-stars accurate. Ratings are social proof — place them near decisions (product cards, testimonials).

## 21.10 Consent / cookie & system banners
A bottom bar or small card: plain-language explanation + clear **Accept / Reject** of equal visual weight (dark-pattern-free — don't hide "Reject"). Non-blocking where possible; remember the choice. The same pattern serves announcement/system banners — keep them dismissible and out of the primary flow.

---

# Part 22 — Expressive typography & signature devices

Ways to make type itself the memorable element (seen in the best marketing designs).

## 22.1 Oversized display type
A very large headline (often one or two words) as the hero's focal point — the "thesis in type." Works when the word is characterful and the rest of the page stays quiet. Tighten tracking hard at these sizes (−0.02 to −0.05em), use `text-wrap: balance`, and let it own the whitespace. This is a stronger, more distinctive hero than the templated "big number + gradient."

## 22.2 Hand-drawn annotations
A scribbled underline, circle, or arrow over a key word adds warmth and personality against clean type (a signature of friendly/human brands). Use once, on the single most important word — more than once and it's noise. Keep it in one color (often the accent) and hand-drawn (imperfect), not a neat vector line.

## 22.3 Structural micro-labels
Small monospace labels — eyebrows, section numbers, `(FIRST) (PRESENT)`, category counts, IDs — that encode real structure (sequence, taxonomy, identifiers). They add an "engineered/considered" texture *when they mean something*. Don't sprinkle `01 / 02 / 03` on content that isn't actually a sequence.

## 22.4 Mixed-weight & mixed-style display
Combining weights/styles within one headline ("Put **people** first" with the key word heavier or annotated) directs emphasis inside the sentence. Keep it to one emphasized word and one contrast axis.

---

# Part 23 — Social proof & trust patterns

Trust is a design deliverable. Common, effective patterns:
- **Logo bar** — "Trusted by" + recognizable customer logos, desaturated/low-opacity so they unify and don't fight the page. Keep to one row.
- **Big-stat blocks** — a few large numbers with small labels ("75.2% daily activity · ~20k users · 4.5 rating"). High-impact, scannable proof. Use tabular figures.
- **Testimonial cards** — quote + name + role + photo/company; keep quotes short and specific; one strong quote beats five generic ones (Peak-End).
- **Ratings & review counts** — near the decision point.
- **Security/compliance badges** — SOC 2, SSO, uptime SLA for B2B/enterprise trust.
Place proof right after the first value claim (post-hero) and again near the final CTA.

---

# Part 24 — Surface & material effects (modern, use with taste)

- **Glassmorphism** — translucent surface + background blur (`backdrop-filter`) + hairline border + subtle shadow. Great for nav bars, floating cards over imagery, and overlays. Requires a busy/colorful background behind it to read; ensure text contrast still passes (add opacity/solidity if not). Don't glass everything.
- **Gradient & mesh backgrounds** — soft, low-contrast multi-stop or blurred "blob" gradients add atmosphere behind heroes and CTAs. Keep hues close and saturation moderate; one gradient moment, not on every surface. (Avoid the AI-default single-blob-behind-dark-hero look.)
- **Grain / noise** — a very low-opacity noise overlay defeats flat sterility and adds a premium, tactile feel — especially over gradients and dark surfaces. Keep it subtle.
- **Glow / accent light** — a soft radial glow behind a key element (a CTA, a metric) draws the eye. One per view.
- **Depth via layering** — overlapping surfaces with consistent shadow/lightness steps read as premium (see Part 7). Combine gently with the above; restraint is the rule.

---

# Part 25 — Theming: light & dark mode with a toggle

Premium products increasingly ship both themes with a user toggle. Do it token-first.

## 25.1 Token-driven theming
Define every color as a **semantic token** (Part 12), then provide two value sets — one under `:root` (light) and one under `[data-theme="dark"]` (or `@media (prefers-color-scheme: dark)`). Components reference tokens only, so the whole UI re-themes by swapping the set. Never hard-code raw colors in components if you support theming.

## 25.2 What actually changes between modes
It's not an inversion. Per mode, re-derive: backgrounds/surfaces (light: white surfaces on light-gray page; dark: layered dark surfaces that get *lighter* with elevation), text (off-black ↔ off-white, never pure), borders (subtle dark line ↔ subtle light line), accent (often slightly lightened + desaturated in dark), shadows (prominent in light ↔ minimal in dark, replaced by lighter surfaces + borders), and any inverted "featured/CTA" section (may flip to the opposite treatment so it still contrasts). Re-check every contrast pair in both modes.

## 25.3 The toggle UX
- A clear control (sun/moon icon, or a labeled switch), usually in the top nav or settings.
- Default to the user's system preference on first load (`prefers-color-scheme`), then let the toggle override.
- Persist the choice across sessions in real products (e.g. `localStorage` or account settings). **Note:** in sandboxed artifact/preview environments, browser storage may be unavailable — fall back to in-memory state (resets on reload) and still honor system preference on load.
- Transition smoothly (a short color transition on `background`/`color`), and respect `prefers-reduced-motion`.
- Set `color-scheme: light dark` so native controls (scrollbars, form fields) match.

## 25.4 Minimal implementation shape
```css
:root{ --bg:#F4F5F8; --surface:#fff; --ink:#0A0E1A; --line:#E7E9EE; --accent:#3346FF; color-scheme:light; }
[data-theme="dark"]{ --bg:#0A0D16; --surface:#121724; --ink:#EEF0F6; --line:#232A3A; --accent:#6D7BFF; color-scheme:dark; }
body{ background:var(--bg); color:var(--ink); transition:background .3s, color .3s; }
```
```js
// default to system, allow override; persist where storage exists
const root=document.documentElement;
const set=t=>root.setAttribute('data-theme',t);
set(matchMedia('(prefers-color-scheme: dark)').matches?'dark':'light');
toggle.onclick=()=>set(root.getAttribute('data-theme')==='dark'?'light':'dark');
```

---

# Part 26 — Style tiles & design-system presentation

Before building screens, produce a **style tile**: a single board showing the palette (named swatches with hex), the type specimen (display/body/mono with weights and sample text), core components (buttons, inputs, chips), radius/spacing/shadow samples, and the signature element. It aligns everyone on the visual language cheaply and is the visual twin of your token system (Part 12). Presenting colors + typography + a component preview together (as the best design files do) is also the fastest way to sanity-check that the direction is cohesive and *not* a default before you commit to full screens.