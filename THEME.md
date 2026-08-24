# Randy & Laurice — Theme & Style Guide

**`index.html` — "The Moment We Say “I Do”", a vintage broadsheet printed in the couple's palette,
in English, with a GSAP motion layer.** Based on `Weddings/samples/newspaper/`.

---

## 1. The idea

A broadsheet printed in **wine-black ink on warm cream stock, resting on emerald cloth**. The mat
behind the paper is what carries the theme at a glance; the ink and stock keep it readable; colour
appears only where it means something.

Photographs run in **full colour**. An earlier draft ran them monochrome, then sepia — both dulled
the couple for the sake of a period effect. The palette already lives in the ink, stock and accents;
the photos don't need tinting to belong.

## 2. Palette

Sampled pixel-by-pixel from `wedding_theme.jpg`.

| Name | Hex | Role |
|---|---|---|
| Emerald | `#005548` | Structure — labels, kickers, subheads, frames, hashtag band |
| Emerald deep | `#00382F` | The mat behind the paper |
| Burgundy | `#801624` | The couple's voice — drop caps, pull quotes, times, links, RSVP |
| Rust | `#A73705` | Small print — FAQ icons, table headers, countdown labels |
| Caramel | `#F37D0F` | The single bright note, on dark bands only |
| Caramel light | `#F9A94A` | Caramel where contrast demands it (see §3) |
| Cream | `#D0AC92` | Swatch, attire chips |
| Wine black | `#1F0206` | The ink |
| Wine mid | `#4E1825` | Deep fills |

### Newspaper roles — all derived from the above

```css
--ink: #1F0206;  --ink-mid: #3D1218;  --ink-light: #6B4038;  --ink-faint: #82624E;
--paper: #F7EFE3;  --paper-mid: #F0E3D2;  --paper-dark: #E5D3BE;
--grey: #A6866E;  --grey-mid: #CBB49E;
--rule: #1F0206;  --rule-thin: rgba(31,2,6,.22);  --mat: #00382F;
```

**No neutral greys anywhere.** Every "grey" is a desaturated warm tone and every shadow is
`rgba(31,2,6,…)`. A cold grey beside a warm photograph is what gives away a re-skin.

## 3. Contrast

All text/background pairs clear WCAG AA (4.5:1). Two needed adjusting when the palette went in —
both small letter-spaced caps, where low contrast bites hardest:

| Pair | Was | Now |
|---|---|---|
| `--ink-faint` on cream | `#9A7A66` — 3.44:1 ✗ | `#82624E` — **4.84:1** |
| RSVP deadline / button on burgundy | caramel — 3.77:1 ✗ | caramel-light — **5.25:1** |

Body ink on stock sits at 17:1. If you change a token, re-check the pair.

## 4. Type

| Family | Job |
|---|---|
| UnifrakturMaguntia | Blackletter mastheads |
| Playfair Display | Headlines, article heads, drop caps, countdown numerals |
| IM Fell English | Italic taglines — the antique-press voice |
| Libre Baskerville | Body copy, tables, itinerary |
| Cormorant Garamond | Body base |
| Jost 200–500 | Small-caps labels, kickers, buttons |
| Brush Script MT (system) | The `R & L` monogram marks |

**Two bugs fixed from the reference sample** — worth knowing if you diff against it:
the font `<link>` carried **two `href` attributes**, so the second silently won and the real font set
never loaded; and `body` declared Cormorant Garamond without ever requesting it.

## 5. Newspaper technique

- Rules: `.rule-thick` / `.rule-double` / `.rule-thin` / `.rule-dashed`; `.rule-ornate` centres a label.
- **Column dividers are real 2px grid tracks** painted `var(--ink)`, not borders. The body and
  bottom grids are `1fr 2px 1.5fr 2px 1fr`.
- Drop caps: `.art-body p:first-child::first-letter`, Playfair 900, floated, in burgundy.
- Justified body with `hyphens:auto`; paper grain from an inline `feTurbulence` SVG.
- **The measure is 2rem** (1rem under 720px). Masthead, headline, front-page plate, body grid,
  hashtag band and footer all inset to it. Anything sitting at 0 reads as a mistake, not a bleed.
- **The hero is a front-page plate**: headline leads, one photograph at the full measure (16:9
  desktop, 3:2 mobile so the couple isn't letterboxed), an italic caption with a credit rule, then a
  four-cell key-facts box ruled like the column dividers. The names are a *caption*, not a hover —
  the previous hover band never appeared on touch and was invisible on desktop until moused.
- **Photo strips: spacing must be `margin-right`, never `gap`.** The marquee translates `-50%`, and
  `gap` is excluded from that measure while margin is included. The sample had both, and the loop
  drifted 7.5px per cycle. Verified at 0.0px.

---

## 6. Motion

GSAP core + ScrollTrigger from cdnjs. Nothing else: `SplitText` and `ScrollSmoother` are **not on
cdnjs**, so word/character splitting is hand-rolled in `splitText()` (~20 lines), and there is
deliberately no smooth-scroll library — it fights native scrolling on mobile.

**The register is editorial, not cinematic.** No pinning, no scroll-jacking, no snapping. Entrances
use `power3.out`, 0.6–0.9s, staggered 0.06–0.09s, and then everything sits still so the page reads
like print.

| Where | Motion |
|---|---|
| Intro | One timeline: sheet unrolls off the cylinder → settles with an elastic overshoot → front page staggers in → roll drops away |
| Handoff | `playMastheadEntrance()` fires at `drop+=0.45`, so the masthead starts *before* the roll finishes falling — no cut between intro and page |
| Masthead | Title split to characters, 0.022s stagger; bars scale outward from centre |
| Front-page plate | Photograph settles from `scale 1.09`, then its caption, then the fact cells tick in left to right |
| Headline | Character stagger, then kicker and sub |
| Column rules | Wipe downward (`scaleY 0→1`, origin top) as each grid enters |
| Articles | Per element — label → head → byline → paragraphs |
| Pull quotes | Rule frame scales in, then the words |
| Swatches | Pop in sequence with `back.out(2)` — the palette introducing itself |
| Countdown | Digits tick over; only the digit that actually changed animates |
| Parallax | Hero photo drifts ±4% inside its frame, `scrub: 1`. That is the only scrub on the page |

### Three contracts that must not be broken

1. **The page works without GSAP.** If the CDN fails, `HAS_GSAP` is false: the intro falls back to
   the original CSS transitions and an IntersectionObserver handles reveals, with a 3-second
   force-reveal timer behind it. Verified by aborting `**/gsap/**` — 0 elements stranded.
2. **`prefers-reduced-motion` is honoured.** A `gsap.matchMedia()` branch sets end states instantly,
   and a CSS block stops the marquees and hands the strips back as ordinary scrollers so photos past
   the fold stay reachable.
3. **Nothing is hidden by default.** Elements are visible in the base stylesheet and only hidden once
   GSAP confirms it is driving. A blocked CDN must never leave a guest on a blank page.

---

## 7. Responsive

`max-width: 720px` — hamburger; the front-page plate goes 3:2 and its fact box becomes 2×2;
`.body-grid` becomes block with columns 2–3 as 49% inline-blocks; `.bottom-grid`
becomes a reordered flex column; roster switches to boxed presentation.
`max-width: 560px` — columns 2–3 stack full width.
