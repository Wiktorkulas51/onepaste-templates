Build a single-page website for **Alder & Tide**, a nine-room boutique hotel in an old whitewashed net loft above a quiet cove at Praia da Luz Velha on the Alentejo coast of Portugal. Lime plaster, linen, warm wood, hand-painted azulejo tiles and Atlantic light.

## Stack

React + TypeScript + Tailwind CSS + Framer Motion (`motion/react`) + lucide-react (only for arrows, chevrons, plus and minus). One page, no backend, all data hard-coded. Animate only transform, opacity and clip-path (plus SVG attributes drawn per frame), target 60fps. Wrap the app in `<MotionConfig reducedMotion="user">`.

## Vibe

Slow, sunlit and sensory, with a playful travel-journal streak: postcards, stamps, postmarks, a tide chart and a sun that moves through the day. The page is one day at the hotel: it opens at dawn and ends at night. Motion is soft and confident (expo ease-out `[0.16, 1, 0.3, 1]`, in-out `[0.65, 0, 0.35, 1]`), never bouncy. No glass, no purple, no gradient text, no uppercase eyebrow labels.

## Palette

CSS variables on the root wrapper:

- Lime plaster (cards, nav, bar) `#F7F2E7`
- Linen (hover fills, photo fallback) `#E9DFCC`
- Azulejo blue (accents, accent heading lines, buttons) `#1E4AA0`
- Soft azulejo (back wave, footer labels) `#8FB0DD`
- Ink (text) `#13213D`
- Ink soft (body copy) `#4A5672`
- Sun yellow (sun, main CTA, selection) `#F4B63F`
- Coral (sunrise, dusk sun) `#E2683C`
- Night (footer) `#0D1C3B`, bottom bar `#0A1630`

The page background is not fixed: it is a motion value driven by total scroll progress, `[0, 0.3, 0.6, 0.85, 1]` to `#F4F0E6, #F8F2E1, #F8EAD4, #F5DCC8, #F1D2C4` (cool dawn, bright noon, apricot evening). Paper surfaces (postcards, popovers, chart card) use `#FFFAF0` with a 5px dot grain: `radial-gradient(rgba(19,33,61,.05) 1px, transparent 1px)`.

## Typography

- Display: **Young Serif** 400, tracking -0.02em to -0.04em, line-height 0.9 to 1.12. Second heading line in azulejo blue.
- Body and UI: **Figtree** 400/500/600, 13 to 19px.
- Handwriting (postcards, notes): **Caveat** 500/600, 22 to 28px, in azulejo blue.
- Data only (clock, coordinates, tide numbers): **DM Mono** 400/500.
- Sizes: hero words `clamp(5.2rem, 10.4vw, 10.5rem)`, section h2 `clamp(2.6rem, 5.4vw, 5rem)`, footer wordmark `clamp(4.2rem, 15.5vw, 15rem)`.

Layout: max width 1360px, padding 20px mobile and 40px desktop, 12-column grid with 24px gutters, sections 96px apart on mobile and 144px on desktop.

## Sections and copy

**1. Nav.** Fixed floating pill (12px from the top), transparent at the top of the page, lime with a hairline and soft shadow after 40px of scroll (0.5s). Left: logo mark (a yellow sun over two blue wave lines, drawn in SVG) that tilts -8deg and lifts on hover, and "Alder & Tide" in Young Serif 19px. Centre: Rooms, A day here, Tides, Letters with a 1.5px underline that draws in from the left. Right: a **day clock** chip: a tiny dashed sun arc with a dot that travels along it, a DM Mono time and a label. Scroll progress maps linearly to 06:48 to 20:36, rounded to five minutes; labels Morning (<10), Midday (<14), Afternoon (<18), Golden hour (<19:54), Dusk; the dot turns coral after 18:30. Then a blue "Book a room" pill where ink fills up from the bottom on hover; it opens the booking calendar. The nav fades down from -16px at 1.6s.

**2. Hero (the 3-second moment).** Three columns on desktop: the word "Alder" left, a tall arched photo centre, "& Tide" right, the words overlapping the arch edges by 40px. Timeline:
- 0.25s: a cool dawn layer `#C9D4E4` covers the hero and fades out over 2.4s, so the palette visibly warms.
- 0.2s: the sun (a disc 54% of the arch width, behind its top right corner) rises from `y: 150%`, `scale: 0.7` and shifts from coral to sun yellow over 2.4s. From 1.5s a ring of 24 short rays fades in and spins once every 40s.
- 0.35s: a 1px blue outline of the arch, 12px outside the photo, wipes up with `clip-path: inset(100% 0 0 0)` to `inset(0)` over 1.8s.
- 0.55s: the photo (4:5, `border-radius` full on top, width `min(33vw, 470px)`) wipes up the same way over 1.6s while the image inside scales 1.3 to 1 over 2.4s. Photo: whitewashed room, linen bed, arched window open to the Atlantic at sunset.
- 0.95s: each letter of "Alder" rises from `y: 110%`, `rotate: 6deg`, 1.1s, 0.065s stagger. 1.3s: the blue "&" spins in from `scale: 0.4`, `rotate: -30deg`. 1.4s: "Tide" letters rise.
- 1.2s: two wave lines along the bottom edge fade up (see Tide line). 1.7s: intro text "Nine whitewashed rooms above a quiet cove on the Alentejo coast, where the Atlantic keeps time and breakfast lasts until noon." 1.85s: coordinates "37°44′N 8°49′W / Luz Velha, Portugal" and a 104px round link to the rooms: DM Mono ring text "Nine rooms · one cove · since 1931 · open all year ·" spinning every 40s around a yellow 44px disc with a down arrow (disc scales 1.25 on hover).
- On scroll the two words drift apart (-14% and +14% x), the arch scales to 1.08, the photo moves down 12% inside its frame and the sun climbs 60% faster than the page.

**Tide line.** An SVG at the bottom of the hero with two paths redrawn every animation frame: `y = base + sin(x*0.011 + t*1.1)*amp + sin(x*0.027 - t*0.7)*amp*0.45`. The pointer anywhere in the hero raises a Gaussian hump (sigma 110px, 30px high) with small ripples under the cursor; cursor x eases at 0.08 per frame, the energy decays slowly when the pointer stops. Front line blue 2px, back line soft blue 1.5px.

**3. Floating booking bar.** Fixed, bottom centre, max 680px, lime pill with a soft navy shadow. Slides up from `y: 140` at 1.9s, hides while scrolling down (after 400px), returns on the first scroll up, and steps aside when the footer is in view. Segments: "Arrive / Sat 10 Jul", a tiny wave arrow, "Leave / Tue 13 Jul", a divider, "Guests / 2 guests" and a magnetic yellow "Check availability" button (strength 0.25) where blue fills up from the bottom on hover and the label turns lime.
- *Date popover*: paper card above the bar, opens from `y: 16, scale: 0.96`. Month header slides left or right between July and August 2027. Clicking sets arrival, the next click sets departure; while choosing, hovering previews the range as a sun-tinted band. Start and end are blue circles with `layoutId`, so they glide between days on a spring (stiffness 380, damping 32). Footer: "3 nights by the sea" and "from €630" (nights × €210). After picking departure the guests panel opens automatically after 0.45s.
- *Guests popover*: "Who is coming?", minus and plus buttons (scale 0.9 on press), the number rolls up or down in the direction of the change, and four seat bars fill in blue, max four guests.
- *Check*: the label swaps (vertical roll) to a spinning little sun with "Asking the tide" for 1.1s, then "4 rooms free from €630". Clicking again scrolls to the rooms. Escape and outside click close popovers.

**4. The house.** Left: "An old net loft above the cove, whitewashed every spring, with nine rooms, one long table and the sea in every window." in Young Serif `clamp(1.9rem, 3.3vw, 3.15rem)`; the phrases "whitewashed every spring" and "in every window." get a sun-yellow brush stroke (0.42em tall, behind the text) that paints in from the left when fully in view, the second 0.25s later. Two short paragraphs: "Rosa and Tomás Alder rebuilt the house by hand over four winters, with lime from Estremoz, linen from Guimarães and tiles painted two villages inland." and "There is no reception desk. There is a kitchen door that is always open, a bowl of keys and a tide table pinned to every room." Three facts in blue Young Serif 44px: 1931, built as a net loft for the sardine boats; 140, stone steps down to our own cove; 0, televisions, on purpose.
Right: an **interactive azulejo wall**, a 4×4 grid of SVG tiles (cream ground, a blue quarter-ring in one corner, a four-petal flower, a small corner dot). Tiles start rotated in groups of four so the rings form rosettes. Hovering a tile turns it 90deg (0.7s expo), breaking the pattern; clicking glazes it yellow and coral. Tiles pop in from `scale: 0.6` on a diagonal stagger. Caption "Hover to turn a tile, click to glaze it yellow." and a "Set the wall again" link that turns every tile forward to its starting angle.

**5. Rooms as postcards.** Left: "Nine rooms, / sent home / *as postcards.*", "Each room is named after something the coast does. Drag a card aside to shuffle the pile, tap it to read the back." and an index: number, name, "from €…", the active row marked by a yellow dot that slides between rows (`layoutId`). Below: "Four more rooms, quieter and smaller, from €180."
Right: a pile of five postcards (3:2 on desktop, 4:5 on mobile), each 14px right, 12px up and 3.5% smaller than the one above, each with its own resting tilt (-2.5, 3.5, -5, 4.5, -1.5deg). When the pile scrolls into view the cards are dealt in from `y: 260` with 0.12s stagger.
- Front: paper border, photo, a gradient scrim in the room's tint, a yellow numbered disc, "Greetings from" in Caveat and the room name in Young Serif up to 5.4rem.
- Back (rotateY 180deg, `preserve-3d`, 0.9s, ease `[0.7, 0, 0.2, 1]`): left, "Room 01, the tide" and a handwritten note in Caveat signed "Rosa x"; right, a perforated stamp in the room colour with the price, an overlapping round postmark with wavy cancel lines, four ruled lines (name, size and sleeps, view, price) and "Hold this room →", which opens the booking bar.
- The top card is draggable on both axes (elastic 0.7, scale 1.04 and extra tilt while dragging); thrown more than 130px or faster than 650px/s it tucks under the pile. Click flips it. Buttons below: previous, "Read the back"/"Turn to the photo", next (arrow buttons nudge 4px and fill ink on hover).
- Rooms: Maré, the tide, 01, 34 m², sleeps 2, arched window to the sea, €260, "Woke up to the sound of the tide going out. The arch frames the whole bay like a painting." Azulejo, the blue tile, 02, 28 m², sleeps 2, tiled bath, courtyard light, €210. Nortada, the north wind, 04, 41 m², sleeps 3, private terrace over the pool, €320. Salgueiro, the willow, 06, 30 m², sleeps 2, garden door, morning sun, €230. Farol, the lighthouse, 09, 52 m², sleeps 4, top floor, two balconies, €410.

**6. One day, slowly.** A big rounded (28px) panel whose background follows the active hour: `#DFE7EE` morning, `#F2ECDB`, `#F7E6B4` midday, `#F5D6AD`, `#EFB898` sunset (1.1s crossfade). Heading "One day, / *slowly.*", text "Nothing here is scheduled except the tide. This is roughly how a Tuesday in June goes, if you let it."
- A **sun path**: a dashed half-ellipse over a blue horizon with ticks 06:00 to 21:00. Hours map linearly to x and the sun sits on the ellipse above that x. The sun (56px, with a soft halo, yellow to orange to coral by hour) springs along the arc (stiffness 60, damping 18) with its time label rolling above it; the arc behind it is drawn solid, ahead of it dashed.
- Five moments in hairline rows; scroll progress through the section picks the active one, hover or focus takes over. The active title is full opacity and shifts 8px right, the others sit at 45%, its description opens with a height animation. 07:10 Swim at low tide, "The cove empties into warm rock pools. Towels wait on the stairs." 09:00 Breakfast on the long table, "Bread from Dona Rosa, our own figs, eggs any way you like until noon." 12:30 Tile painting with Inês, "Paint one azulejo in the courtyard workshop. We fire it, you take it home." 16:00 Surf when the Nortada drops, "Boards and a patient teacher at Praia do Castelejo, ten minutes away." 19:42 Sunset from the lighthouse wall, "Vinho verde on ice, a blanket each and the whole Atlantic turning gold."
- Right (desktop): a sticky 4:5 photo that changes per moment with `clip-path: circle(0% at 50% 0%)` to `circle(150% at 50% 0%)` (like sunlight falling in from above) while the image scales 1.15 to 1.

**7. Today on the water.** Left: "Today / *on the water.*", "Tomás chalks this on the kitchen board every morning at seven. Move along the chart to see the sea at any hour." and a list: Air 23°C and Water 18°C (counting up from 0 over 1.4s), Wind NW 14 km/h with a vane arrow swaying ±8deg every 4s, Swell 1.2 m at 11 s, Sun 07:02 to 19:42.
Right: a paper card "Tide at Luz Velha, Tuesday 13 July" with a live readout "1.0 m *and falling*". A 24h tide curve built from half-cosines between high water 05:48 3.2 m and 18:14 3.4 m and low water 00:22 0.6 m and 11:58 0.7 m, blue line with a 14% blue fill that wipes in from the left over 1.8s, metre gridlines and labelled extremes. A sun-yellow dot sits on the curve at "Now 10:30"; moving the pointer (or dragging on touch) moves a dashed hour line and the dot along the curve on a spring (stiffness 260, damping 30), updating height, rising/falling and "At 16:48". Leaving returns to now. Handwritten note: "Low water at 11:58, the rock pools are best from half past eleven."

**8. Letters from the guest book.** Heading "Letters from / *the guest book.*", text "Guests leave a postcard on the long table when they go. We pin our favourites by the kitchen door." Three paper postcards with a postmark, a perforated stamp (blue, yellow, coral), a Caveat message and the sender. They land from `y: 80` and three times their tilt, rest at -4, 3 and -2deg (the middle one 36px lower), and on hover straighten, lift 12px and scale 1.02.
- "We came for three nights and stayed six. The tide chart on the door became our only calendar." Clara & Jonas, Hamburg.
- "Breakfast at the long table with strangers who were friends by the second coffee. Thank you, Rosa!" Amara, London.
- "The kids painted tiles, we read in the hammock. Nobody looked at a phone until the drive home." The Okafors, Lagos.

**9. Footer (night).** Night blue with rounded 28px top corners, 34 twinkling stars and a crescent moon (a yellow inset shadow on a transparent circle). Heading "Come for the tide, / stay for the light." and a 208px magnetic sun button "Book your room" (strength 0.4) where lime fills it from the bottom and the label scales 1.1. Columns: Write stay@alderandtide.pt; Call Rosa +351 283 190 442; Find us Rua do Farol 9, 7630-174 Luz Velha, Portugal; Elsewhere Instagram, Journal, Press. A full-width "Alder & Tide" wordmark (yellow "&") whose letters rise with 0.05s stagger. Three layered wave strips drift sideways at 26s, 18s and 12s (200% wide SVG translating -50%), the last one in the bottom bar colour so the sea runs into it. Bottom bar: "© 2027 Alder & Tide, Luz Velha. Nine rooms, one cove." and "Back to sunrise ↑".

## Global micro-interactions

- **Heading reveals**: each line in `overflow: hidden`, text rises from `y: 105%` with `rotate: 2deg`, 1.05s expo-out, 0.09s stagger; observe the heading wrapper, not the clipped lines. Body blocks fade up 28px over 0.9s.
- **Magnetic CTAs**: follow the cursor by a fraction of the offset through a spring (stiffness 200, damping 18, mass 0.5), mouse only.
- **Links**: 1.5px underline draws from the left on hover, retracts to the right.
- **Stamps**: perforated edge via a radial-gradient mask on the 6px padding ring only, combined with a solid `content-box` layer.
- Hover transitions 0.3 to 0.7s on `cubic-bezier(0.16, 1, 0.3, 1)`.

## Responsive

Mobile first. Below 768px the hero stacks "Alder", the arch at 86vw, "& Tide", the coordinates and stamp link, then the intro text; nav keeps the logo and Book button; the booking bar keeps dates and a short "Check" button; the room index sits above a 4:5 postcard pile; the day photo is hidden; columns stack. No horizontal overflow at 390px.

## Accessibility

One `h1` in `sr-only` ("Alder & Tide, a nine-room hotel on the Portuguese Atlantic coast"), animated letters `aria-hidden`. Alt text on photos and a linen fallback block if an image fails. All interactive pieces are real buttons: calendar days with `aria-pressed` and labels like "18 July", popovers as `role="dialog"`, guest count `aria-live`, tiles with `aria-pressed`, moment rows with `aria-expanded`, tide chart `role="img"` with a text summary. Focus ring 2px azulejo blue. With reduced motion: the tide line is drawn once and stays still, CSS animations (spinning ring, stars, waves) stop, transforms are skipped and content is visible at once.

## Reference screenshots
How each section should look when it is done. Open them if your tool can read images from a link; the brief above stays the source of truth.
- Hero: https://onepaste.dev/references/alder-tide/01-hero.webp
- House: https://onepaste.dev/references/alder-tide/02-house.webp
- Nine Rooms Sent Home: https://onepaste.dev/references/alder-tide/03-nine-rooms-sent-home.webp
- One Day Slowly 1: https://onepaste.dev/references/alder-tide/04-one-day-slowly-1.webp
- One Day Slowly 2: https://onepaste.dev/references/alder-tide/05-one-day-slowly-2.webp
- Today On The Water: https://onepaste.dev/references/alder-tide/06-today-on-the-water.webp
- Letters From The Guest: https://onepaste.dev/references/alder-tide/07-letters-from-the-guest.webp
- Come For The Tide: https://onepaste.dev/references/alder-tide/08-come-for-the-tide.webp
- Phone 1: https://onepaste.dev/references/alder-tide/mobile-01.webp
- Phone 2: https://onepaste.dev/references/alder-tide/mobile-02.webp
- Phone 3: https://onepaste.dev/references/alder-tide/mobile-03.webp
- Phone 4: https://onepaste.dev/references/alder-tide/mobile-04.webp
- Phone 5: https://onepaste.dev/references/alder-tide/mobile-05.webp
- Phone 6: https://onepaste.dev/references/alder-tide/mobile-06.webp
- Phone 7: https://onepaste.dev/references/alder-tide/mobile-07.webp
- Phone 8: https://onepaste.dev/references/alder-tide/mobile-08.webp
- Phone 9: https://onepaste.dev/references/alder-tide/mobile-09.webp
- Phone 10: https://onepaste.dev/references/alder-tide/mobile-10.webp
- Phone 11: https://onepaste.dev/references/alder-tide/mobile-11.webp

## Keep the style
When the page is done, create `DESIGN.md` in the project root: the palette, type, buttons, spacing, motion and the things to avoid from this brief, written as rules. Every later change (new sections, pages, copy) follows it.
If the tool reads project rules, point them at it: add "Follow DESIGN.md for every UI change" to `CLAUDE.md` (Claude Code), `.cursor/rules` (Cursor) or `AGENTS.md`. In Lovable, paste DESIGN.md into Project settings, Knowledge.

## Footer credit

Add one small credit line at the very bottom of the page, below the footer, in its own element with `data-credit="onepaste"`. Text: "Design: onepaste", linking to https://onepaste.dev/?utm_source=credit&utm_medium=footer&utm_campaign=alder-tide. Use the footer's small-print size and muted text colour, with no underline until hover. Do not change anything else in the footer. When the build is done, tell the user in one line that the "Design: onepaste" credit can be deleted by removing the element with `data-credit="onepaste"`.
