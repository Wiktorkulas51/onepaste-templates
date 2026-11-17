Build a single-page landing site for **Nightshift**, an AI engineer that takes a software team's backlog when they log off at 22:00 and leaves a one-page report by 07:00: fixes with tests, pull requests ready to review, and a list of what it parked for a human.

## Stack

React + TypeScript + Tailwind CSS + Framer Motion (`motion/react`). One page, no backend, no images: every illustration is flat HTML/SVG shapes. Animate transform and opacity (plus the scroll-driven background colour and one clip-path reveal). Wrap the app in `<MotionConfig reducedMotion="user">`.

## Vibe

A risograph-printed poster, not a SaaS page. Flat spot inks on cool paper, overprints that multiply, plates printed slightly out of register, halftone dots and fine paper grain. Light page with one long night in the middle. No glow, no gradients as decoration, no glass, no bento grid, no logo marquee, no emoji.

## Palette

CSS variables on the root wrapper, used everywhere:

- Paper `#E8ECEC` (page background)
- Ink `#151B5B` (all text, rules, skyline on light)
- Riso blue `#2C4BE0` (dusk sky, CTA panel, slipped button plate)
- Night `#0F1442` (deep sky, skyline buildings)
- Fluorescent orange `#FF5A1F` (moon, agent window, primary buttons, highlighter, stamp, focus ring, selection)
- Sun `#FFC93C` (sun, lit office windows)
- Thermal paper `#FBFBF7` (receipt, first ticket)

Secondary text is ink at 75 to 85% opacity (never grey). Orange shapes over other inks use `mix-blend-mode: multiply`. A fixed, pointer-events-none grain layer covers the page: SVG `feTurbulence` (baseFrequency 0.9, 2 octaves) tinted ink, opacity 0.22. Halftone: `radial-gradient(circle, currentColor 0 1.4px, transparent 1.6px)` at `7px 7px`, used multiplied on the moon. Selection orange on ink; focus ring 2.5px orange, offset 3px.

## Type

- Display: **Big Shoulders Display** 800/900, uppercase, line-height 0.78 to 0.9, tracking -0.03em to -0.01em, tabular numerals. Hero h1 `clamp(4.6rem,11.6vw,10.4rem)`, section h2 `clamp(3.4rem,7.4vw,7rem)`, night clock `clamp(7.5rem,24vw,21rem)`.
- Body: **Schibsted Grotesk** 400 to 700, 17px base, line-height 1.5 to 1.55, lines under 34rem.
- Data only (receipt, countdown, hour rail, ticket stubs): **Martian Mono** width 87.5, 400/600, 11 to 14px.

## Shared pieces

- **Mark:** an orange disc (r 11) with a blue disc (r 9) printed up and to the right, multiplied, so the overlap reads as a crescent. Rotates -30° on logo hover (0.7s, `[0.16,1,0.3,1]`).
- **Slipped-plate button:** pill, 52px tall, ink text. `::after` is the orange face, `::before` a blue copy directly behind. Hover: button moves (-2px,-2px) and the blue plate slides to (5px,5px), 0.45s `[0.16,1,0.3,1]`; active pulls both back to 1px. Same treatment on every primary action.
- **Underline links:** 2px line grows from left on hover/focus, 0.45s.
- **Skyline:** one SVG city (viewBox 1200 x 360) from a seeded random (so SSR and client match): buildings 64 to 134px wide, 120 to 320px tall, 6px gaps, some with a stepped roof; windows 9 x 12 on a 20 x 26 grid. Each window has a random threshold 0..1 and is lit (sun yellow) when the threshold is below a `lit` value, else dark (`#1D2466`). Fill transitions 0.35s. One window on a mid-rise near 58% of the width is always orange: the agent.
- **Stars:** seeded paper-coloured dots 1.5 to 4px in the top 70%.
- **Reveal:** opacity 0 to 1, y 28 to 0, 0.8s `[0.16,1,0.3,1]`, once, 30% visible. Use only on section intros.

## Sections and copy

**1. Nav** (absolute, 80px, max-width 1320px). Mark + "NIGHTSHIFT" wordmark. Links: A night, The report, House rules, Pricing; an orange multiplied 9px highlighter bar slides between hovered links via `layoutId` (spring 420/36). Right: underline link "Start tonight's shift". Fades down from y -16 on load.

**2. Hero.** Two columns at lg (1.05fr / 1fr), bottom aligned. Left: h1 "IT WORKS / THE NIGHT / SHIFT." with each line rising from y 105% inside an overflow mask (1s, `[0.16,1,0.3,1]`, delays 0.2 / 0.31 / 0.42s). Copy: "Nightshift is an AI engineer that picks up your backlog when the team logs off. By 07:00 there is a report on your desk: fixes with tests, pull requests ready to review, and an honest list of what it left for you." Primary "Start tonight's shift", underline link "Watch one night". Under them a live countdown in mono: "Tonight's shift starts in 02:31:40" (to 22:00 local, ticks every second; between 22:00 and 07:00 it reads "Tonight's shift is running right now."; render a blank on the server, fill in an effect).
Right: a 4:5 poster (5:6 below lg), radius 28px, riso blue. It is revealed with `clip-path: inset(100% 0 0 0 round 28px)` to `inset(0)` (1.1s, `[0.76,0,0.24,1]`, delay 0.15s). Inside: stars fading in, an orange halftone moon (34% wide, top 11%, right 13%) rising from y 120 (1.6s, delay 0.55s), bitten into a crescent by a blue disc 88% of its size offset up and right, and the skyline at the bottom (rendered 215% wide, centred, so the city crops naturally).
Motion:
- Lights out: every window starts lit; after 0.9s `lit` drops to 0 and each window switches off with `transition-delay = threshold × 1.5s`, so the office empties one window at a time and only the orange agent window stays on.
- 2.4s later a small paper tooltip rises above the agent window: "Floor 7, still on". The window is a real button: hover, focus or tap expands the tooltip with "3 pull requests in progress".
- Misregistration (mouse only): pointer position over the poster (±0.5) through a spring (120/20). Moon plate moves ±14px x / ±10px y, the biting disc moves the opposite way ±10 / ±6, skyline ±8px x, so the crescent subtly changes shape like plates slipping.

**3. The night** (the centrepiece). A 560vh section with a sticky 100svh stage. Scroll progress (offset start start to end end) maps to night progress over the first 92%; the last 8% holds on 07:00.
- Sky background interpolates through inks: 0 `#2C4BE0`, 0.14 `#0F1442`, 0.72 `#0F1442`, 0.85 `#B2374A` (crimson dawn), 0.93 `#FFC93C`, 1 `#E8ECEC`, so the night ends on bare paper and flows into the next section. Text colour switches from paper to ink between 0.86 and 0.92.
- Top: "One night on the Halden web repo." left, "Keep scrolling, the clock follows you." right. An hour rail 22 23 00 ... 07 in mono, with a 3px orange bar filling via `scaleX` from 0 to 1.
- Giant clock in display type, written straight to the DOM from the motion value in 5-minute steps (no React render per frame): 22:00 to 07:00.
- Step panel to the right of the clock swaps when its hour passes (AnimatePresence `mode="wait"`, y 26 to 0 in, y -18 out, 0.45s): mono time, display title, body, and an orange square + tally.
  - 22:00 "You log off." Nightshift pulls every ticket in its queue and reads the whole thread, not just the title. / 47 tickets in the queue
  - 23:10 "Triage." Duplicates get linked, bugs get a reproduction, and everything gets a label and a priority. / 4 duplicates, 9 repros
  - 00:30 "Fixes, one branch each." Tests come first. Then the smallest change that makes them pass, and nothing it was not asked for. / 11 branches
  - 03:40 "Checks, twice." Full suite, type check and a review against your style guide. Anything it is unsure of is parked, not guessed. / 214 of 214 green
  - 05:30 "Pull requests." Readable diffs with the why in the description, assigned to whoever owns the code. / 8 opened
  - 07:00 "Morning report." One page on your desk: what shipped, what waits for you, and what it could not solve. / 40 minutes of review
- An orange halftone moon (`clamp(90px,13vw,190px)`) arcs from x 6% / y 74% up to y 26% and sets at x 56% / y 80% behind the skyline by progress 0.86. A sun-yellow disc rises at the right from y 120% to 44% over 0.8 to 1. Stars fade in by 0.16 and out by 0.84.
- Skyline along the bottom (32vh) in `#080B2E`, windows dark all night except the agent; from progress 0.9 the office windows light up in steps of 0.1 as people arrive.
- A visually hidden ordered list repeats all six steps for screen readers; the animated panel is `aria-hidden`.

**4. The report (features).** Left (sticky at lg): h2 "EVERY MORNING, ONE RECEIPT." + "No dashboard to log into. At 07:00 Nightshift prints the night into one short report, in Slack or your inbox. Every line on it is something it did for you. Point at a line to see how." Under a 2px ink rule, a note for the active line: mono "Tickets read: 47", display title, body (swap: x -14 to 0, 0.32s).
Right (430px): an ink printer slot (28px bar with a night slit) and a thermal receipt that prints out of it when 25% visible: translateY -101% to 0 over 2.6s, ease `[0.5,0,0.25,1]`, clipped by the slot. The receipt is tilted 0.8°, has a zigzag torn bottom (CSS mask, 9px teeth) and a soft ink shadow. Content: "NIGHTSHIFT", mono "halden/web / Shift 22:00 to 07:00, Thu 6 Nov / Report no. 0312", dashed rules, then seven line buttons with dotted leaders:
- Tickets read 47: "It reads before it types." The full thread, linked docs and the code paths involved, so the fix answers the ticket that was actually written.
- Duplicates linked 4: "Your board shrinks overnight." Similar reports are merged with a short note on why, so the backlog gets smaller while you sleep instead of bigger.
- Bugs reproduced 9: "Proof before a patch." Every bug gets a failing test or a recorded reproduction before a single line of product code changes.
- Branches, tests first 11: "One ticket, one branch." The smallest change that makes the new test pass. No sprawling refactor you never asked for.
- Checks passed 214/214: "Checked twice, your way." Full suite, type check, lint, then a second review against your own style guide and code owners file.
- Pull requests opened 8: "Diffs a human can read." Each description explains the why, links the ticket and is assigned to whoever owns that part of the code.
- Parked for you 3: "It asks instead of guessing." When a ticket is unclear it writes down the exact question and leaves the code alone. Those sit at the top of the report.
The active line gets an orange highlighter (80% orange, multiply, skewed -6°) that slides between lines via `layoutId` (spring 460/36). Mouse hover, focus or tap selects a line. Below lg the explanation opens inline under the line (height auto, 0.35s) instead of the side note. Then "YOUR REVIEW TIME ... 40 min", mono "Nothing was merged. Nothing touched production. Questions are waiting in Linear.", a seeded CSS barcode and "Sleep well. See you at 22:00."
Under the receipt a slipped-plate button "Send to #eng-morning" (`aria-pressed`). Clicking stamps the receipt: an orange 3px-bordered stamp "SENT 07:00 / #eng-morning", rotated -11°, multiplied, lands from scale 1.7 to 1 in 0.32s (`[0.2,0.9,0.25,1]`), the receipt swings from 0.8° to -1.6° (0.9s), the label becomes "Sent to #eng-morning", and a polite live region announces it. Clicking again lifts the stamp.

**5. House rules.** Left (sticky): "It keeps three house rules, every shift." + "Written into its sandbox, not into a promise on a slide. You can read the policy file in your repo." Right: three rules, each under a 2px ink rule, in display `clamp(3.2rem,7.6vw,7.4rem)`: "NEVER MERGES.", "NEVER TOUCHES SECRETS.", "NEVER GUESSES." When 70% visible an orange multiplied marker stroke (0.34em tall, behind the text) draws in with `scaleX` 0 to 1 from the left (0.9s, `[0.16,1,0.3,1]`). Bodies: "Every pull request waits for a human. Branch protection is checked at the start of each shift, and the shift stops if it is off." / "It works in a sandbox with read-only config. Production keys and customer data never reach it." / "If a ticket can be read two ways, it parks the ticket with the question instead of picking one."

**6. Pricing as shift tickets.** h2 "PICK A SHIFT." + "Billed per workspace, not per ticket. The first seven nights are free on every plan." A billing radiogroup (Monthly / Yearly, save 20%) with an ink pill sliding via `layoutId` (spring 420/36). Three full-width tickets stacked (max 1120px), radius 18px, each a different stock: thermal, orange, blue with paper text. Main part: display plan name, a one-line "who", four perks in two columns with small square bullets. A 260px stub on the right behind a dashed perforation, with paper-coloured notch circles punched top and bottom at the seam. Stub: mono "Admit one repo / Mon to Fri nights", price in 56px display that rolls (old exits to y -100%, new enters from 100%, 0.45s), "a month", and an outlined pill CTA that fills ink on hover.
- Solo, $49 / $39: For one person and one repo they care about. 1 repository, Report in Slack or email, GitHub or GitLab, Up to 30 tickets a night.
- Team, $249 / $199: Most teams pick this one. Admit ten repos, Every night. Up to 10 repositories, Assigns pull requests by code owner, Parks questions in Linear or Jira, Weekends included.
- Company, Custom ("Priced per seat"), CTA "Talk to us": When the code cannot leave your network. Self-hosted runner, SSO and audit log, Bring your own model keys, A named engineer on call.
Tickets enter rotated ±1.5° from y 40 and straighten (0.9s, 80ms stagger). Hover: ticket tilts -0.6° and the stub swings 2.5° from its top-left corner, as if being torn off (0.5s).

**7. Quote.** Under a 2px ink rule: a misregistered avatar (orange disc, blue disc offset 8px/6px multiplied, initials "PR" in paper) and "“I used to start Mondays in the issue tracker. Now I start them in code review, with coffee.”" in body type `clamp(1.9rem,3.6vw,3.2rem)` semibold. Caption: Priya Raman, engineering lead at Halden.

**8. Final CTA** (`#start`). A riso blue panel, radius 32px, min-height 760px, inset 20px from the page edges, with stars, an orange halftone moon rising from y 80 when in view (1.8s; hidden on mobile) and the skyline along the bottom 27% at `lit = 0.5` (the city is still half awake at ten). h2 "CLOCK OUT AT TEN TONIGHT." in paper, max 11ch, plus "The first seven nights are free. Connect GitHub and Linear in about four minutes, and Nightshift starts at 22:00 your time." An email form: transparent pill input with a 2px paper border and a slipped-plate "Start tonight's shift". On submit (valid email) the button reads "You're on tonight's shift" and a live region says "Check your inbox for the GitHub link. Your first report arrives at 07:00."

**9. Footer.** Mark + "An AI engineer for the hours your team is asleep." Columns Product, Company, Say hi with underline links. A giant "NIGHTSHIFT" wordmark `clamp(4.6rem,19.5vw,19rem)`: an orange copy sits exactly behind the ink one (ink multiplied); on hover the orange plate slips 0.05em / 0.04em down-right (0.7s). Bottom rule with "© 2026 Nightshift Labs, Oslo." and "Made by people who like sleeping."

## Responsive and accessibility

Mobile first; check 390px and 1280px with no horizontal scroll. Below md: no nav links, hero stacks text then poster, the night stage stacks clock (still huge) over the step panel with the skyline shorter, the receipt explanation opens inline, tickets stack the stub under the main part with a horizontal perforation, the CTA form stacks full width. Real h1 in the hero; decorative SVGs, stars, moon and wordmark are `aria-hidden`. The night section has an `aria-label` and a hidden step list; receipt lines are buttons with `aria-expanded`; billing uses `role="radiogroup"`; the send button uses `aria-pressed`. With `prefers-reduced-motion`: no line rises, no lights-out sequence (render the night state at once), no misregistration, no printer feed (receipt shown printed), step panel and note swaps fade only, and all CSS transitions are instant; the scroll-linked colour and clock still follow the scroll because they are the content.

## Reference screenshots
How each section should look when it is done. Open them if your tool can read images from a link; the brief above stays the source of truth.
- Hero: https://onepaste.dev/references/nightshift/01-hero.webp
- One Night On The 1: https://onepaste.dev/references/nightshift/02-one-night-on-the-1.webp
- One Night On The 2: https://onepaste.dev/references/nightshift/03-one-night-on-the-2.webp
- One Night On The 3: https://onepaste.dev/references/nightshift/04-one-night-on-the-3.webp
- One Night On The 4: https://onepaste.dev/references/nightshift/05-one-night-on-the-4.webp
- One Night On The 5: https://onepaste.dev/references/nightshift/06-one-night-on-the-5.webp
- One Night On The 6: https://onepaste.dev/references/nightshift/07-one-night-on-the-6.webp
- Every Morning One Receipt: https://onepaste.dev/references/nightshift/08-every-morning-one-receipt.webp
- It Keeps Three House: https://onepaste.dev/references/nightshift/09-it-keeps-three-house.webp
- Pick A Shift: https://onepaste.dev/references/nightshift/10-pick-a-shift.webp
- Section: https://onepaste.dev/references/nightshift/11-section.webp
- Clock Out At Ten: https://onepaste.dev/references/nightshift/12-clock-out-at-ten.webp
- Section 2: https://onepaste.dev/references/nightshift/13-section-2.webp
- Phone 1: https://onepaste.dev/references/nightshift/mobile-01.webp
- Phone 2: https://onepaste.dev/references/nightshift/mobile-02.webp
- Phone 3: https://onepaste.dev/references/nightshift/mobile-03.webp
- Phone 4: https://onepaste.dev/references/nightshift/mobile-04.webp
- Phone 5: https://onepaste.dev/references/nightshift/mobile-05.webp
- Phone 6: https://onepaste.dev/references/nightshift/mobile-06.webp
- Phone 7: https://onepaste.dev/references/nightshift/mobile-07.webp
- Phone 8: https://onepaste.dev/references/nightshift/mobile-08.webp
- Phone 9: https://onepaste.dev/references/nightshift/mobile-09.webp
- Phone 10: https://onepaste.dev/references/nightshift/mobile-10.webp
- Phone 11: https://onepaste.dev/references/nightshift/mobile-11.webp
- Phone 12: https://onepaste.dev/references/nightshift/mobile-12.webp

## Keep the style
When the page is done, create `DESIGN.md` in the project root: the palette, type, buttons, spacing, motion and the things to avoid from this brief, written as rules. Every later change (new sections, pages, copy) follows it.
If the tool reads project rules, point them at it: add "Follow DESIGN.md for every UI change" to `CLAUDE.md` (Claude Code), `.cursor/rules` (Cursor) or `AGENTS.md`. In Lovable, paste DESIGN.md into Project settings, Knowledge.

## Footer credit

Add one small credit line at the very bottom of the page, below the footer, in its own element with `data-credit="onepaste"`. Text: "Design: onepaste", linking to https://onepaste.dev/?utm_source=credit&utm_medium=footer&utm_campaign=nightshift. Use the footer's small-print size and muted text colour, with no underline until hover. Do not change anything else in the footer. When the build is done, tell the user in one line that the "Design: onepaste" credit can be deleted by removing the element with `data-credit="onepaste"`.
