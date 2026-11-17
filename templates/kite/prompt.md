Build a single-page landing site for **Kite**, a playful money app for freelancers: get paid in any currency, auto-set aside taxes, send instant invoices, spend with a holographic card. All names and numbers are invented.

## Stack

React + TypeScript + Tailwind CSS + Framer Motion (`motion/react`) + lucide-react. One page, no backend. Animate transform and opacity (plus the FAQ height, SVG stroke offsets and background positions on the card). Wrap the app in `<MotionConfig reducedMotion="user">`.

## Vibe

Joyful, colourful, tactile, confident. Saturated tangerine, lilac and mint blocks, crisp near-black type, chunky rounded shapes and UI pieces that look like die-cut stickers: 2.5px ink border plus a hard offset shadow (`4px 5px 0 ink`), slightly rotated. Every section has its own flat colour field. No dark navy, no glows, no gradients on surfaces (only the holo card), no glass, no emoji, no fake logo strips.

## Palette

- Paper `#FFF5EA` (base), Ink `#141217` (type, borders, shadows)
- Tangerine `#FF6A13` (hero, primary CTA, liquid), Lilac `#C6B0FF` / soft `#ECE3FF`, Mint `#93EDC3` / soft `#DDFAEB`, Butter `#FFE15A`, Pink `#FF9CC5`
- Faint dot grid on colour fields: `radial-gradient(ink 18% 1.2px, transparent 1.4px)` at 22px.
- Selection: ink background, butter text. Focus ring: 3px ink outline, 3px offset.

## Type

Google Fonts. Display **Bagel Fat One** (chunky, rounded, sticker-like), letter-spacing -0.01em, line-height 0.86 to 0.92. Body and UI **Gabarito** 400 to 900; labels 800 uppercase with 0.12em tracking. Hero `clamp(3.5rem,9.4vw,7.4rem)`, h2 `clamp(2.6rem,6vw,5rem)`.

## Buttons

Pills with 2.5px ink border and a solid `0 5px 0 ink` shadow. Hover: translateY(-2px), shadow 7px. Active: translateY(4px), shadow 1px, so they press like physical keys (0.18s, ease `[0.22,1,0.36,1]`). Primary CTAs are wrapped in a magnetic wrapper: the button follows the cursor by 30% of the offset from its centre through a spring (stiffness 280, damping 17), mouse only, springs back on leave.

## Images

- `https://onepaste.dev/images/kite/card.webp`: 3:2 render of a blank holographic card on grey. Use it only as a texture: `background-size: 300% auto`, position about `55% 40%` so only the iridescent foil is visible.
- `https://onepaste.dev/images/kite/hand.webp`: 4:5 photo of a hand with lilac nails holding a blank phone on tangerine. App screens are HTML overlaid on the screen area (top 8.9%, left 34.2%, width 39.9%, height 66.4%, radius `13cqw`), the overlay is a size container so all screen UI is sized in `cqw`.

## Sections

**1. Nav.** Fixed, top 12px, max-width 72rem. Transparent over the hero; after 40px of scroll it becomes a paper sticker bar (ink border, hard shadow). Logo: diamond kite mark split into tangerine, lilac and mint quarters plus the wordmark "kite" in Bagel; on hover the mark lifts 4px and rotates -14°. Links: Invoices, Currencies, Tax pot, Pricing; a butter pill with ink border slides between hovered links (`layoutId`, spring 420/32). Right: "Log in" text link and magnetic ink "Get Kite". Slides down from y -100 on load (0.8s).

**2. Hero** (tangerine, 100svh, dot grid). Left: headline in three lines "Invoice it. / Get paid. / Go fly.". Behind "Get paid." a butter rounded highlight. Sub: "The money app for freelancers. Bill clients in 38 currencies, get paid in a day, and let the tax pot quietly keep what was never yours." CTAs: magnetic ink "Open a free account" and paper "See it work ↓". Proof row: four overlapping initial avatars in lilac, mint, butter, pink and "★ 4.9 from 31,400 freelancers".
Right: the holographic card, rotated -7°, with three stickers around it: a paper notification "Studio Nord paid you +€2,400.00" with a mint € tile (top left, -5°), a lilac "Tax pot, auto $3,412 28%" with a progress bar (bottom left, 4°), and a 128px butter round badge "0%" with "0% FX markup • 38 currencies •" on a textPath rotating every 16s (bottom right). Top right: an illustrated kite (tangerine, lilac and mint quarters, ink outlines, bow tail in butter and pink) bobbing on a 3.4s loop.

Holo card build: `aspect-ratio 1.586`, radius `6% / 9.5%`, layers bottom to top: foil texture (position follows the pointer 44 to 66% x, 30 to 50% y, `hue-rotate(±40deg) saturate(2.1)`); a repeating 115° rainbow stripe gradient (pink, butter, mint, sky, lilac) at 55% opacity with `mix-blend-mode: color-dodge`, its background-position following the pointer; a white radial glare centred on the pointer, once with soft-light and once with overlay at 60%. Print in ink: "kite" wordmark, contactless icon, gold chip, "•••• 4021", "MARA OKAFOR", "DEBIT". A blurred ink ellipse shadow under the card shifts ±30px opposite the pointer.

Hero motion:
- Load: headline lines rise from y 110% and rotate 6° to 0 inside overflow masks (0.95s, `[0.22,1,0.36,1]`, delays 0.2/0.32/0.44s); highlight scales X from 0 at 1.05s (0.7s). Sub at 0.7s, CTAs at 0.85s, proof row at 1.1s.
- Card flips in from rotateY -100°, rotateZ -24°, y 160, scale 0.7 to its resting pose (spring 70/14, delay 0.35s), then bobs y -10px every 5s.
- Stickers slap in from scale 0.3 with a -24° or +20° overshoot (spring 420/15) at 1.25s, 1.45s, 1.65s; the tax bar fills to 28% at 1.8s.
- Pointer over the hero (mouse only) is normalised to ±0.5 through a spring (120/16). Card rotateY ±24°, rotateX ±18°. Stickers parallax ±18 to 22px, some with and some against the cursor.
- Without a pointer the card keeps drifting on sine waves (x period about 9.4s, y about 13s, amplitude 0.22/0.14), so the foil shimmers in silent recordings. Disabled with reduced motion.
- On scroll the card stage drifts down 140px over the hero height.

**3. Kite string** (desktop ≥1200px). An SVG overlay over the whole page draws a 2.25px ink string from the hero kite's tail down the right gutter as a gentle S-curve with ±16px sway, ending at the footer. The visible length follows scroll: its tip sits at 72% of the viewport height (dash offset from a sampled length/height table). A small butter bow rides on the tip.

**4. Invoice to paid** (paper). Left: kicker sticker "Invoices", h2 "Invoice to paid / in one tap.", copy "Pick a client, hit send and watch it land. Kite converts at the real rate and drops the money straight into your balance. Go on, press the button." Then a 3-step status list with 48px icon tiles: Sent (just now), Opened (2 sec later), Paid (landed in your balance). Tiles turn ink (Paid turns mint) with a 1.18 scale pop and -8° wiggle as each step is reached.
Right: a lilac-soft stage (radius 36px, dot grid). Top right: paper "Balance, USD, all clients" sticker with a rolling-digit odometer starting at $12,480.40. Client radio chips with a sliding ink pill: Studio Nord (Copenhagen, €2,400, "Brand motion kit, 6 loops"), Pebble & Co (Austin, $1,850, "Landing page illustrations"), Mori Inc. (Osaka, ¥240,000, "App icon set, 24 icons"). Invoice sticker card "Invoice #0142 · Due in 7 days" with a header in the client's tint, bill-to line and the big amount. Tangerine "Send invoice" button.
Click flow: invoice flies off to the top right (x 420, y -260, rotate 28°, scale 0.4, 0.65s, ease `[0.5,0,0.75,0]`); a spinning send badge with "Flying to your client…" (0.65s), "{client} is looking…" (1.35s), then Paid at 2.15s: the badge turns mint with a check springing in (500/14), "landed as $2,608.70", and a butter sticker "28% moved to your tax pot, $730.44 set aside" slaps in. Twelve "$" coins burst from the invoice and arc into the balance (x ease `[0.55,0,0.75,0.35]`, y ease `[0.2,0.85,0.3,1]`, scale 0 → 1.25 → 0.6, random spin, 50ms stagger, 0.8s); 0.9s later the balance odometer rolls up and the balance sticker pops (scale 1.08, rotate -2° → spring back). Button becomes "Send another", which picks the next client. The demo plays once by itself 1.4s after the stage is 55% in view, unless the visitor already interacted.

**5. Currencies** (lilac). Kicker, h2 "Get paid in 38. / Hold 12. Swap in a blink." A paper sticker panel: "YOUR BALANCE" and a huge Bagel odometer of $18,240.50 converted live (EUR 0.92, GBP 0.79, JPY 151.3 with 0 decimals, PLN 3.97, BRL 5.05). Segmented control USD EUR GBP JPY PLN BRL with a sliding pill in the currency's colour. Rate line under it swaps vertically (0.35s): "1 USD = 0.92 EUR · mid-market, no markup". Auto-cycles every 2.4s while in view until the visitor picks one. Four big round currency stickers (€ £ ¥ R$) float in the margins (desktop), bob on 4 to 5.4s loops, scale 1.12 on hover and switch the currency on click. Three stat stickers below: "1 day", "0.4%", "12", lifting and tilting -1° on hover.

Odometer: every digit is an overflow-clipped 1em column holding 0 to 9 stacked; translateY(-digit × 10%) with `transition: transform .9s cubic-bezier(.2,.9,.2,1.04)` and a 35ms stagger from the right. Columns are keyed from the right so units stay aligned when the length changes; an invisible copy of the current digit sizes each column so narrow digits leave no gap. Expose the full value with `aria-label`.

**6. Tax pot** (mint). Kicker, h2 "Taxes? Already / set aside.", copy "Every payment that lands, Kite skims the tax slice into a pot you cannot accidentally spend. Drag to see what a month looks like." Sticker panel with a native range input "Monthly income" $1,000 to $20,000, step 250, default $9,000 (18px ink-bordered track filled ink up to the thumb, 38px tangerine thumb with hard shadow that scales 1.15 and rotates 8° on hover). Rate = 18% + 14% × position. Tiles "Into the pot" and "Yours to spend" (ink tile, mint number), both odometers.
Right: a jar sticker (radius 56/64px) with a tangerine lid, filled with tangerine liquid whose level springs (70/13) to 12 to 80% of the jar. The surface is two SVG waves scrolling at 5s and 3.2s; dragging fast tilts the surface up to ±9° through a bouncy spring (120/6) that settles back. Bubbles rise inside. A paper tag "24% kept for taxes" rides on the surface; the yearly total "$25,800 / a year, already sorted" sits in the liquid. Butter round sticker "No more April panic" on the lid.

**7. The app** (paper, 400vh on desktop). Sticky stage: left a progress rail and four steps (01 Home "Every client, one balance.", 02 Invoices "Invoices in 20 seconds.", 03 Tax pot "Taxes, set aside on autopilot.", 04 Card "A card that keeps up."); inactive steps fade to 32%, the active one shows its paragraph (height auto, 0.4s). Right: the hand photo in a sticker frame tilting ±2.5° per step, screens swapping inside the phone (enter y 18% and scale 0.96, exit y -10% and scale 0.94, spring 210/24). Screens: Home (balance $18,240, currency pills, 7 bars springing up with 50ms stagger, three transactions sliding in), Invoice (lilac, €2,400, a "PAID" stamp slamming from scale 2.4 and -30° to -12°), Tax pot (mint, ring filling to 72% in 1.2s, "$3,412"), Card (ink, mini holo card flipping in from rotateY 90°, a working "Freeze card" toggle that greys the card). Below 1024px: no sticky; tabs switch the screens.

**8. Sticker wall** (butter, dot grid). H2 "31,400 freelancers, / zero spreadsheets." and a hint chip "Peel them off, move them around". Six testimonial stickers (four rounded rectangles, two circles) in white, lilac, mint, pink, paper and tangerine, scattered with -9° to 8° tilts. They slap in with a spring (340/15, 80ms stagger). On desktop they are draggable inside the board (elastic 0.18); the last touched one goes on top, while dragging it scales 1.1, straightens a little and casts a deeper 12/16px shadow. Hover: scale 1.04, rotate 0, lift 6px. Below 768px they stack in a grid without dragging. Example: "I invoiced a client in Tokyo from a train in Lisbon and got paid before my stop." Inès Moreau, Motion designer.

**9. Pricing** (paper). H2 "Pick your altitude.", "Start free. Upgrade when the clients pile up. Cancel in two taps." A chunky switch Monthly / Yearly ("2 months free" chip): mint track, paper knob that slides 36px and spins 180° (spring 500/26). Three sticker cards tilted -1.5°, 0, 1.5° that straighten and lift 8px on hover: Glide $0 (paper), Soar $9 or $7 (tangerine, "Most loved" butter tag, ink CTA), Stratos $19 or $15 (lilac). Prices are odometers; caption "billed $84 a year" or "billed monthly".

**10. FAQ** (paper). Sticky h2 "Fair questions." and "Still curious? Chat with a human in the app, usually in under four minutes." Accordion of sticker rows; the open one tints (lilac, mint, butter, pink), its plus button turns ink and rotates 135° (spring 400/20), the answer opens with height (0.42s). Questions: Is Kite a bank?, How fast do clients pay me?, How does the tax pot decide how much to keep?, What does it cost to convert currencies?, Can I cancel anytime?

**11. Footer** (ink, 48px top corners). "Go fly / a kite." with "kite." in tangerine, copy "Open an account in four minutes. Your first invoice can go out before your coffee cools.", magnetic tangerine "Get Kite free →" with a paper key shadow. Three link columns (Product, Company, Help) whose links slide 4px and turn mint on hover. A giant tangerine "kite" wordmark `clamp(7rem,33vw,28rem)`: letters rise in with a 70ms stagger, on hover each jumps 10%, tilts ±6° and changes colour. The illustrated kite loops across the wordmark every 11s (x from -12% to 108%, wavy y, tilt -12° to 12°) while a paper string, recomputed every frame as a quadratic curve, connects it to the bottom left. Legal line: "© 2026 Kite Money Ltd. Kite is not a bank. Banking services by Harbor Trust Bank."

## Responsive

Desktop first at 1280px, grid sections collapse to one column below 1024px. Hero headline sits above the card on mobile; stickers stay attached to the card. The kite string and margin currency stickers are hidden below 1200px and 1024px. Phone tour becomes tabs. Sticker wall becomes a grid. Keep tap targets at least 44px.

## Accessibility

Real buttons, `role="radiogroup"`/`radio` for client and currency pickers, `role="switch"` for the billing toggle, `aria-expanded` and `aria-controls` on the FAQ, `aria-live="polite"` on balances, labelled range input, alt text on the hand photo, decorative SVGs `aria-hidden`. With `prefers-reduced-motion`: no idle drift, no CSS loops (waves, bobbing, spinning badge, bubbles), odometers jump, dragging is disabled, the string is drawn in full, the invoice flow skips the flight and coins.

## Reference screenshots
How each section should look when it is done. Open them if your tool can read images from a link; the brief above stays the source of truth.
- Hero: https://onepaste.dev/references/kite/01-hero.webp
- Invoice To Paid In: https://onepaste.dev/references/kite/02-invoice-to-paid-in.webp
- Get Paid In 38: https://onepaste.dev/references/kite/03-get-paid-in-38.webp
- Taxes Already Set Aside: https://onepaste.dev/references/kite/04-taxes-already-set-aside.webp
- Every Client One Balance 1: https://onepaste.dev/references/kite/05-every-client-one-balance-1.webp
- Every Client One Balance 2: https://onepaste.dev/references/kite/06-every-client-one-balance-2.webp
- Every Client One Balance 3: https://onepaste.dev/references/kite/07-every-client-one-balance-3.webp
- Every Client One Balance 4: https://onepaste.dev/references/kite/08-every-client-one-balance-4.webp
- 31 400 Freelancers Zero Spreadsheets: https://onepaste.dev/references/kite/09-31-400-freelancers-zero-spreadsheets.webp
- Pick Your Altitude: https://onepaste.dev/references/kite/10-pick-your-altitude.webp
- Fair Questions: https://onepaste.dev/references/kite/11-fair-questions.webp
- Go Fly A Kite 1: https://onepaste.dev/references/kite/12-go-fly-a-kite-1.webp
- Go Fly A Kite 2: https://onepaste.dev/references/kite/13-go-fly-a-kite-2.webp
- Phone 1: https://onepaste.dev/references/kite/mobile-01.webp
- Phone 2: https://onepaste.dev/references/kite/mobile-02.webp
- Phone 3: https://onepaste.dev/references/kite/mobile-03.webp
- Phone 4: https://onepaste.dev/references/kite/mobile-04.webp
- Phone 5: https://onepaste.dev/references/kite/mobile-05.webp
- Phone 6: https://onepaste.dev/references/kite/mobile-06.webp
- Phone 7: https://onepaste.dev/references/kite/mobile-07.webp
- Phone 8: https://onepaste.dev/references/kite/mobile-08.webp
- Phone 9: https://onepaste.dev/references/kite/mobile-09.webp
- Phone 10: https://onepaste.dev/references/kite/mobile-10.webp
- Phone 11: https://onepaste.dev/references/kite/mobile-11.webp
- Phone 12: https://onepaste.dev/references/kite/mobile-12.webp

## Assets
Use these hosted files while you build, so the page matches the preview. Before you publish, download them into the project (or swap in your own) and update the paths.
- https://onepaste.dev/images/kite/card.webp
- https://onepaste.dev/images/kite/hand.webp

## Keep the style
When the page is done, create `DESIGN.md` in the project root: the palette, type, buttons, spacing, motion and the things to avoid from this brief, written as rules. Every later change (new sections, pages, copy) follows it.
If the tool reads project rules, point them at it: add "Follow DESIGN.md for every UI change" to `CLAUDE.md` (Claude Code), `.cursor/rules` (Cursor) or `AGENTS.md`. In Lovable, paste DESIGN.md into Project settings, Knowledge.

## Footer credit

Add one small credit line at the very bottom of the page, below the footer, in its own element with `data-credit="onepaste"`. Text: "Design: onepaste", linking to https://onepaste.dev/?utm_source=credit&utm_medium=footer&utm_campaign=kite. Use the footer's small-print size and muted text colour, with no underline until hover. Do not change anything else in the footer. When the build is done, tell the user in one line that the "Design: onepaste" credit can be deleted by removing the element with `data-credit="onepaste"`.
