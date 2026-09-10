# Content Checklist — Randy & Laurice

**Current build: `index.html` — "The Moment We Say “I Do”" broadsheet, in English.**

**The names, dates, venue and story are now the real ones**, taken from `R_L_Wedding_Plan.xlsx`
(sheets `ENTOURAGE` and `STORY`). Anything the plan did not supply reads **`Pending`** on the page
rather than a fake value. Find what is still outstanding:

```bash
grep -n "Pending" index.html
```

### Still `Pending` — needs the couple

Nothing outstanding — every field the plan left blank now has a value on the page.

**One correction to the plan.** The `ENTOURAGE` sheet has principal sponsor row 7 as
*Mary Jone G. Yap* under **Mr.** and *Asterio B. Yap* under **Ms.**, the reverse of every other pair.
Confirmed as a transcription slip and swapped on the page: Asterio under Gentlemen, Mary Jone under
Ladies. The spreadsheet itself still has it the other way round.

---

## 1. Do this first — the constants at the top of the `<script>`

```js
var WEDDING_DATE = '2026-10-30T14:30:00+08:00';   // countdown target, keep the +08:00 offset
var MAPS_URL     = 'https://maps.app.goo.gl/59h7Bm5nqG49pVAh9';   // the ceremony
var IMG          = 'assets/img/';                 // leave alone
```

`WEDDING_DATE` is the **ceremony** time, 2:30 PM — that is what the countdown counts down to.

`MAPS_URL` drives the QR **and** the button beneath it, and points at the **ceremony** — the first
place guests have to find. The "Getting There" subtitle says so, and the reception has its own link
on its venue card.

The two `View Directions ↗` links inside the venue cards are separate `href`s and now carry
**different** URLs, one per venue. There are three map links in total: ceremony card, reception card,
and `MAPS_URL`.

⚠️ The ceremony link is the Google Maps shortlink the couple supplied
(`maps.app.goo.gl/59h7Bm5nqG49pVAh9`, with its `?g_st=ic` app-tracking param stripped). It has never
been opened from this repo — **tap it once on a phone** before the invitation circulates.

---

## 2. The masthead and mastheads-within

The paper's identity appears in **two** places and they should agree:

| | Intro roll | The page |
|---|---|---|
| Title | `.np-masthead` | `.mast-title` |
| Location / date | `.np-date-row` | `.mast-top-bar` |
| Headline | `.np-headline` | `.hl-main` |
| Dateline | `.np-lede-dateline` | `.hl-byline` |
| Contents | `.np-contents-list` | the nav links |
| RSVP / hashtag | `.np-notice` | `.rb-deadline` |

Also: `<title>`, the `og:` meta tags, and the three placements of the monogram —
`.nav-mark` in the nav, `.nf-mark` in the footer, and `.sig-logo` above the signature in the
closing message. All three draw `assets/img/np-logo.png`, masked to a circle.

---

## 3. Names

All of these started from the `ENTOURAGE` sheet — 51 people — and the couple has since added to the
list directly.

| Field | Value |
|---|---|
| Bride / Groom | Michelle Laurice A. De Joya / Engr. Randy W. Odchigue |
| Bride's parents | Mr. Rey Arnel S. De Joya & Mrs. Eulogia A. De Joya |
| Groom's parents | Mr. Efrino G. Odchigue & Mrs. Eva W. Odchigue |
| Best Man / Maid of Honour | John Rey A. De Joya / Mellen Angelie W. Odchigue |
| Groomsmen & Bridesmaids | 5 rows each — add or remove `<tr>`s freely |
| Principal Sponsors | 12 rows |
| Officiant | *removed* — the sponsors column reads "Chosen to guide us". To reinstate, put the name back in that `.art-byline`. |
| Secondary sponsors | ring / coin / bible bearers, flower girls, little brides, candle / cord / veil sponsors, offertory |

Titles (`Mr.` / `Ms.`) were dropped from the roster lines — the plan lists bare names, and several
secondary sponsors have no title given.

**Secondary sponsors are written twice** — once as `.desktop-roster` lines and once as
`.mobile-sponsors-boxes`. `applyMobileView()` swaps them at 720px. Edit **both** or they'll disagree
between phone and desktop.

---

## 4. Dates, times, venues

| Field | Value | Appears in |
|---|---|---|
| Wedding date | ✅ Friday, October 30, 2026 | `WEDDING_DATE`, mast top bar, front-page fact box, headline byline, intro dateline, venue cards, closing block, archive table |
| Ceremony | ✅ 2:30 PM | front-page fact box, programme, venue card, FAQ, reminders |
| Reception | ✅ 5:00 PM | front-page fact box, programme, venue card |
| Guest arrival | ✅ Before 2:30 PM (FAQ) | FAQ |
| Programme | ✅ 2:30 PM–10:00 PM, five slots from the timeline card | programme |
| RSVP deadline | ✅ October 10, 2026 | front-page fact box, RSVP box, intro notice |
| City | ✅ Cebu City | mast bar, front-page caption, bylines, venue cards, closing block, intro dateline |
| Ceremony venue | ✅ Archdiocesan Shrine of St. Thérèse, Lahug, Cebu City | front-page fact box, programme, venue card, intro lede |
| Reception venue | ✅ Golden Peak Hotel & Suites, Gorordo Avenue corner North Escario Street, Cebu City | front-page fact box, programme, venue card, intro lede |
| Hashtag | `#GodUnitedRandyAndLaurice` | front-page fact box, hashtag banner, intro notice, FAQ |

The spreadsheet listed only Golden Peak, under both the ceremony and reception times. The couple
confirmed the ceremony is at the **Archdiocesan Shrine of St. Thérèse** in Lahug and the reception at
**Golden Peak** — so the two venues are now separate everywhere they appear. The `ENTOURAGE`/`STORY`
plan still shows only the one.

The front-page fact box says **"Shrine of St. Thérèse"** rather than the full name; the box is narrow
and the full title wraps to three lines there. The venue card and programme carry it in full.

**Timeline dates** from the `STORY` sheet feed the archive table: became a couple **June 20, 2024**,
proposal **September 29, 2025**, wedding **October 30, 2026** — which makes "Years Together" read
**2 years**.

---

## 5. Copy to make theirs

The prose is now the couple's own, lifted from the `STORY` sheet and split across the two article
blocks so neither column runs away with it.

- **"Faith Lead Us Here"** — the first half of their story, from strangers through
  *"Hi, dayun ta laag?"* (the pull quote) to Ecclesiastes 3:11 (the second pull quote).
- **"One Last Invitation"** — the closing half: the grateful-hearts paragraph, the bigger yes, and
  *"This is our story. This is our answered prayer."*
- **The intro sheet's lede** (`.np-lede-body`) — the plan's one-line invitation plus the time and
  venue. Also used verbatim as the headline sub.
- **"Notes from the Archive"** — six rows now. First met and first date are `Pending`; the rest are
  real.
- **Notes & Reminders** — gifts (GCash/bank are `Pending`), unplugged ceremony, punctuality
  (2:30 PM), adults-only reception. **The adults-only note is a real policy decision** — delete it if
  it doesn't apply.
- **FAQ** — five questions. Arrival time and parking answer `Pending`; check the other three are
  actually true for this wedding.

### The dress box runs on the couple's own card

Both images in the box are crops of the same dress-code card, so the palette, the outfits and the
attire chips are one set of colours rather than three:

| | |
|---|---|
| `np-palette.jpg` | the six swatches, above `.db-text` |
| `np-dresscode.jpg` | the eight figures, between `.db-text` and the attire grid |

Both keep the card's cream field (`#F8E4CC`), which is a shade warmer than `--paper`. That is
deliberate — it is what makes them read as a matched pair from one printed card instead of two stray
images.

The palette, sampled from the card:

| Name on the page | Hex |
|---|---|
| Sage Green | `#BAB076` |
| Olive Green | `#4A481F` |
| Chocolate Brown | `#5A3216` |
| Burgundy | `#5F1917` |
| Caramel | `#AB6631` |
| Cream | `#E8C49E` |

⚠️ **These are garment colours and have nothing to do with `:root`.** The paper's own ink still uses
`--emerald #005548`, `--burgundy #801624` and the rest for labels, rules and frames; those must not be
changed to match the dress code. The only places the garment colours appear are `np-palette.jpg`,
`.db-text`, and the five `.attire-swatch-inline` chips.

⚠️ **The groomsmen's colour was renamed.** It read "Emerald Green" with a teal chip, which matched
neither the card nor the suits pictured below it; it now reads **Olive Green** against `#4A481F`. If
the entourage was told "emerald", change the word — but change it with the chip, not on its own.

The card's own summary line ("burgundi + green + caramel") is cropped out of `np-palette.jpg` on
purpose: the colour names are already in `.db-text`, and the line carries a typo.

Nothing was invented to fill a gap. Where the plan is silent, the page says `Pending`.

---

## 6. RSVP — connected

The form posts to the Apps Script web app in [`apps-script/Code.gs`](apps-script/Code.gs), which
writes each reply to the **`RSVPs`** tab of the RSVP spreadsheet. `RSVP_ENDPOINT` at the top of the
RSVP block in the inline `<script>` holds the live `/exec` URL. If it is ever emptied, the form still
validates and shows its success state so it can be demoed, and logs a warning to the console — but
replies go nowhere.

### How it is wired (Apps Script → Google Sheets)

1. A Google Sheet holds the replies.
2. **Extensions → Apps Script**, and replace the contents with [`apps-script/Code.gs`](apps-script/Code.gs),
   then run `setup()` once from the editor and grant the permissions it asks for. That creates the
   **`RSVPs`** tab and its header row:
   `Timestamp · Name · Attending · Seats · Message · Submitted (browser) · Revisions`

   The script validates and clamps what arrives (name required, attending must be one of the two
   values, seats forced to 0 on a decline and capped at 2, message truncated at 400), takes a lock so
   simultaneous replies cannot collide on a row, and — with `UPDATE_EXISTING` on — overwrites a
   guest's earlier row when they reply again instead of leaving two. `CONFIG` at the top also carries
   an optional RSVP cutoff date and a token for the `?action=stats` read.

3. **Deploy → New deployment → Web app.** Execute as **Me**; who has access **Anyone**.
   (It must be "Anyone", or guests get a sign-in wall.)
4. Copy the **`/exec`** URL and paste it into `RSVP_ENDPOINT`:

   ```js
   var RSVP_ENDPOINT = 'https://script.google.com/macros/s/AKfy…/exec';
   ```

5. Submit one test reply and confirm the row lands in the sheet.

**Why the request is shaped the way it is:** the form posts with `URLSearchParams`, which keeps it a
"simple" request so the browser skips the CORS preflight that Apps Script cannot answer. Change it to
JSON with a `Content-Type: application/json` header and submissions will start failing CORS. The
values arrive on `e.parameter`.

### Fields collected

| Field | Notes |
|---|---|
| `name` | Required |
| `attending` | Required — "Joyfully accepts" / "Regretfully declines" |
| `guests` | 1–2; sent as `0` automatically when they decline. Capped at 2 on purpose — an invitation may name a couple, but never an extra guest on top of the names printed on it, which is what the "Strictly no +1" line beside the field means. Widening the `<select>` re-opens that contradiction. |
| `message` | Optional, capped at 400 characters |
| `submitted` | ISO timestamp from the guest's browser |

The seats dropdown hides itself when someone declines, the success copy differs for accept vs.
decline, and a failed send keeps everything typed so nobody has to fill it in twice.

### Reusable custom dropdown

Native `<select>` panels render with OS chrome — system font, blue highlight — which breaks the
broadsheet. Any select can opt into the themed one:

```html
<label class="rb-label" id="mealLabel" for="meal">Meal preference</label>
<select id="meal" name="meal" data-custom-select>
  <option value="chicken">Chicken</option>
  <option value="fish">Fish</option>
</select>
```

`data-custom-select` is the whole API. `enhanceSelect()` builds a themed listbox beside the element
and mirrors every choice back onto it, firing a normal `change` event — so existing form code keeps
reading `select.value` and nothing else needs to know. Give the `<label>` an `id` and the trigger
picks it up via `aria-labelledby`.

- **The native `<select>` stays in the DOM.** If the script never runs, the form still submits.
- Keyboard: ↑/↓ move, Enter/Space select, Esc closes and returns focus, Home/End jump, type-ahead
  by first letter.
- ARIA: `role="combobox"` + `aria-expanded` on the trigger, `role="listbox"`/`option` in the panel,
  `aria-activedescendant` while open.
- Touch targets are 47px on mobile — it replaces the OS picker, so it has to be as easy to hit.

### Gifts

Gift details in the "On Gifts" note read `GCash: Pending` and `Bank: Pending` — the plan does not
carry them.

---

## 7. Photos — done

| Folder | What | Deploy? |
|---|---|---|
| `assets/photos/` | 31 early originals at 2048px **+ 53 full-resolution camera files** (6240×4160, ~1 GB) | ❌ No — archive only, and **gitignored** |
| `assets/img/` | web-sized crops | ✅ Yes |

Newspaper set (`np-*`, ~30 MB on disk, 265 files). Disk size is not page weight: the gallery ships
four rungs of every frame and a browser downloads exactly one of them. A full read of the page costs
**~3.4 MB on desktop and ~3.0 MB on a phone**.

| File | Where | Size |
|---|---|---|
| `np-cover.jpg` | Photo on the rolled intro paper — the bamboo-hut canopy shot | 760×440 |
| `np-palette.jpg` | Swatch strip at the top of the dress box | 900×155 |
| `np-dresscode.jpg` | Illustration inside the dress box | 900×696 |
| `np-share.jpg` | Link-preview card (`og:image`) | 1200×630 |
| `np-hero.jpg` | Front-page plate, **frame 1** | 1440×810 |
| `np-hero2–hero5.jpg` | Front-page plate, frames 2–5 | 1440×810 |
| `np-c1–c8.jpg` | Carousel, 16:9 | 900×506 |
| `np-s1–s18.jpg` | First photo strip | 560×400 landscape / 280×400 portrait |
| `np-r1–r18.jpg` | Second strip (reversed) | same |
| `np-g01–g53-400.jpg` | Grid tile — phones, and desktop at 1× | 400px wide |
| `np-g01–g53-640.jpg` | Grid tile — desktop at 2× | 640px wide |
| `np-g01–g53-1000.jpg` | Viewer — phones | 1000px long edge |
| `np-g01–g53-1600.jpg` | Viewer — desktop | 1600px long edge |

The `type` (`landscape` / `portrait`) in the `PHOTOS` and `PHOTOS_REVERSE` arrays must match the
crop's actual shape — it sets the item's width. A portrait file in a landscape slot will stretch.

Both strips run **18** photos now, up from 10. The scroll duration has to move with the count: a strip
travels exactly one copy of its set per cycle, so leaving 120s in place while adding eight photos would
simply have made the crawl 80% faster. `scroll-strip` is now **216s** and `scroll-strip-reverse` **108s**
— the reverse strip is meant to run at twice the pace, which is why it is half, not equal.

### The picture supplement — `#gallery`

53 frames, the couple's own running order: `np-g01` is their first pick and `np-g53` their last. The
source files were named `1IMG_…` through `8IMG_…` by rank, unprefixed meaning last; that ranking is
baked into the `np-gNN` numbering, so **to re-rank, re-cut the files — the page just counts upward.**

**It opens on 12 frames, not 53.** `GALLERY_PREVIEW` at the top of the gallery script sets that, and
the button below the grid swaps between the twelve and the full set. Laid out in full the supplement
was the longest thing on the page — a wedding invitation should not make you scroll past a contact
sheet to reach the RSVP. Twelve fills roughly one screen in either column count, and the viewer still
pages through all 53 starting from any of them, so nothing is actually hidden. The soft fade at the
foot of a collapsed grid is the only cue that it is cut, which is why the mask sits on `.gallery-grid`
and not on the section — it has to end where the photographs end, not where the button does.

**Every frame ships in four sizes and the browser picks one.** The grid tile carries
`srcset="…-400.jpg 400w, …-640.jpg 640w"` plus a `sizes` string that describes the real column width
(two columns inside 1rem padding and a 7px gap on a phone; three inside the 960px sheet, 1.6rem
padding and two 10px gaps above that). Get `sizes` wrong and the saving evaporates — a phone will
happily download the 640px file to paint it 175px wide, which is exactly what it did before. The
viewer does the same with its 1000/1600 pair at `sizes="100vw"`.

Together with the 12-frame preview that is the difference between a **6.5 MB** page and a **3.4 MB**
one; on a phone, between 6.1 MB and 3.0 MB, and a tapped frame costs ~130 KB instead of ~390 KB.

Three more things that are not arbitrary:

- **The grid is laid out in JS, not with CSS `columns`.** Multi-column fills straight down column one
  before it starts column two, which would have buried the couple's best-ranked frames at the foot of
  the first column. Each photo goes into whichever column is currently shortest, so the ranking reads
  *across* the top row the way a contact sheet does. Three columns above 720px, two below.
- **The viewer is a sibling of the paper, not a child of the gallery.** `.gallery-section` carries
  `.reveal`, and `.reveal` sets a transform — a transformed ancestor makes `position: fixed` resolve
  against *it* rather than the viewport, which would have pinned the overlay inside the section.
- **`w`/`h` in the `GALLERY` array are the 640px file's real dimensions.** They become the `<img>`
  attributes, so each tile reserves its height before the bytes arrive, and they are also what the
  column balancer measures. Wrong numbers mean both a jumping page and lopsided columns. They are
  only ever read as a *ratio*, so they stay correct no matter which rung the browser fetches.

Tiles also carry `content-visibility: auto`, which lets the browser skip layout and paint for frames
scrolled well away — it matters most with the grid expanded to 53. The paired `contain-intrinsic-size`
is what keeps the scrollbar from lurching while they are skipped.

Escape closes the viewer, ← and → step through and wrap, Tab is trapped inside the dialog, and on a
phone a horizontal swipe flips frames. Opening a frame freezes the page behind it at its scroll
position and restores it on close. Neighbouring frames are preloaded so ‹ and › feel instant — except
on a connection that reports `saveData` or 2G/3G, where fetching three photographs to show one is the
wrong trade.

⚠️ The hero plate and carousel crops were checked for the same treatment and **left alone on purpose**:
they are already efficiently encoded. Re-compressing `np-hero3.jpg` at q45 bought 16% and cost visible
quality, and `np-c1.jpg` at q50 bought 5%. There is nothing to win there.

### The front-page plate cycles

`#heroPlate` cross-fades through its four frames on a 5.2s hold and wraps back to the first. To change
the set, add or remove `.hp-frame` images inside the figure — **DOM order is play order**, and the code
counts them, so there is nothing else to update.

Three things about it that are not arbitrary:

- **Frame 1 is the base and never fades.** It carries no opacity rule at all, so a blocked script, a
  reduced-motion setting or a frame that fails to download still leaves a photograph in the plate.
  Frames 2–4 are overlays that fade in *above* it.
- **Wrapping drops all overlays at once** rather than fading them one by one. An earlier frame cannot
  show through a later one that is still up — DOM order is also stacking order.
- **Frames 2–4 are `alt=""` + `aria-hidden`.** They are the same subject as frame 1; announcing four
  photographs of the same couple to a screen reader is noise, not information.

It skips a turn — holding the current frame for another full 5.2s — while the cover is still up (so
frame 1 gets its moment once the paper drops) and while the next frame is still downloading (a fade to a
half-loaded image shows nothing, then pops). It stops entirely off-screen and in a background tab.

The plate runs **five** frames. `np-hero5.jpg` and the current `np-cover.jpg` are two crops of the same
new photograph — the couple on the canopy bed in the bamboo hut — cropped 16:9 and 1.73:1 respectively
from one 2048×1365 original, both anchored to the top edge so the canopy peak survives and the trim
comes off the bedding.

⚠️ **That original is not in the repo.** `assets/photos/` is gitignored, so the only copies of it here
are the two derived crops. The same warning now covers ~1 GB of full-resolution camera files sitting in
that folder — they are the source for every `np-g*` frame and every `np-s11`–`s18` / `np-r11`–`r18` crop,
and **nothing in git has a copy of them.** Back that folder up. Keep the full-resolution file wherever the rest of the originals live —
without it there is nothing to re-crop from.

`np-share.jpg` is a third crop of the same original, at 1.9:1. That one is nudged **70px down**
rather than top-anchored — the card is much wider than the cover, so a top anchor left a band of
empty roof above the couple.

`np-hero.jpg` and `np-c5.jpg` are the same photograph — that predates this build, but it is the kind of
repeat worth avoiding if more crops get made.

All photographs run in **full colour**. Earlier drafts tinted the strips monochrome and then sepia;
both dulled the couple, and the palette already carries the theme through the ink, stock and accents.

---

## 8. Music — done

**Track:** `assets/music/goodness-of-god.mp3` — *Goodness of God*, Bethel Music. 4.5 MB, 5:04.

It arrived as `Goodness Of God (Lyrics)  Bethel Music.mp3`; **renamed** because spaces — and a *double*
space — in a deployed URL are a needless thing to debug on somebody's phone. To swap the track, drop the
new file in `assets/music/`, point the `<source src>` at it, and retype `#music-label`.

`hasTrack()` still guards everything: blank the `src` and the player removes itself rather than showing
a dead button.

### Starting on the cover tap

`unroll()` calls `playMusic()` as its **first** statement, before it touches the animation — that tap is
the gesture browsers require, and it has to be spent synchronously to count. The keyboard path
(Enter/Space on the cover) goes through the same `unroll()`, so it starts the music too.

Behind that sit `click` / `scroll` / `touchstart` fallbacks on `document`, each `{once: true}`, for a
guest who reaches the page without tapping the cover — the 20-second failsafe unroll, mainly, which has
no gesture behind it and will be refused. `playMusic()` claims `started` synchronously so the cover tap
doesn't fire a second `play()` when the same click bubbles to those fallbacks, and the rejection handler
sets it back to `false` so a refused attempt hands the fallbacks their job again.

If the very first attempt is refused *and* it was a real click that consumed the fallback, the guest gets
no music until they press the record themselves. That is the intended floor — the player is visible and
says what it does.

### The player

A newsprint clipping pinned bottom-right, sitting a little crooked, carrying a 45 that turns while the
track plays. Two states, no third:

| | Closed | Open |
|---|---|---|
| When | paused | playing, or hovered |
| Shows | the record alone | kicker, title, four bars of ink |

**It must not depend on hover.** The old label was `#music-player:hover` only, which on a phone meant a
guest never learned what they were listening to. The `.open` class is what fixes that, so if the panel
stops opening on touch, that class is where to look.

Two rules that are load-bearing, both of which were bugs first:

- **The face follows the `<audio>` element, not the `play()` promise.** `setPlay()` runs from the `play`
  and `pause` events. That promise can take seconds to settle on a slow connection, and a guest who taps
  a record that neither spins nor names the song concludes it is broken and taps again — which pauses it.
- **`.mp-meta` spacing is `padding-left`, not a flex `gap`.** Padding collapses with the panel under
  `max-width: 0`; a gap does not, and leaves 8px of dead paper hanging off the closed card.

`prefers-reduced-motion` stops the record and freezes the bars part-height. The volume is `.45` in
`playMusic()`.

⚠️ **4.5 MB downloads for nearly every guest**, because the intro tap starts playback. `preload="none"`
keeps it off the initial page load, but not off their data. Re-encoding to ~96 kbps mono would cut it to
under 1 MB with no audible loss at background volume — worth doing before the invitation goes out.

---

## 9. What not to touch

- The `:root` ink/paper variables and the rule classes.
- The grid template `1fr 2px 1.5fr 2px 1fr` (body, bottom). The `2px` tracks *are* the column dividers.
- The front-page block's `2rem` measure — it must match the masthead, headline, body grid and footer.
- The intro unroll, carousel, countdown, FAQ, QR, strips, and `applyMobileView()`.
- **How the intro sheet fits.** `#paper-sheet` is a fixed `85vh`, so the front page has a hard height
  budget. The type is constant and the *plate* absorbs the difference: `.np-photo-section` is
  `flex: 1 1 auto; min-height: 0` and `.np-photo` is `max-height: 40vh; min-height: 60px`. Three rules
  hold this together, and each one was a bug first:
  - `min-height: 0` on the section — without it the automatic minimum is the image's natural height,
    and the colophon prints straight through the inner border on a short screen.
  - `min-height: 60px` on the image — if the plate stops shrinking before its box does, the *caption*
    spills onto the lede.
  - `max-height` in `vh`, not `px` — a pixel cap leaves a hole inside the bordered plate on a tall
    screen, because the section grows and the image doesn't.
  Below `700px` of viewport height the lede hides; below `560px` the contents rule goes too. Change any
  of this and re-check at 1000 / 800 / 760 / 667 / 560 px tall, and at 320px wide.
- **`.np-contents` and "2 Pages"** in `.np-date-row` refer to each other. Adding a sixth entry to the
  contents means the date row should stop claiming two pages.
- The two head fixes carried over from the sample — a single-`href` font `<link>`, and Cormorant
  Garamond actually being loaded. Reverting either silently kills the typography.
- The music `hasTrack()` guard and the intro's 20-second failsafe.
- **Photo-strip spacing.** The marquees translate `-50%`, which only loops seamlessly because spacing
  is `margin-right` on `.photo-strip-item` and there is **no `gap`** on `.photo-strip-track`. The
  sample had both and drifted 7.5px per cycle. Verified at 0.0px.

**The motion layer** (GSAP core + ScrollTrigger). Three load-bearing rules:

1. `HAS_GSAP` — if the CDN fails, the intro falls back to CSS transitions and an IntersectionObserver
   handles reveals, with a 3-second force-reveal timer behind it. Never assume GSAP is present.
2. **Nothing is hidden in the base stylesheet.** Elements only become invisible once GSAP confirms it
   is driving, so a blocked CDN cannot leave a guest on a blank page.
3. The `prefers-reduced-motion` branch and its CSS block, which also hands the strips back as
   ordinary scrollers so their photos stay reachable.

⚠️ **When editing the inline script by search-and-replace:** the stylesheet contains section comments
with the *same names* as the script's (`/* ── NAV ── */`, `/* ── COUNTDOWN ── */`). Scope the match to
the `<script>` block or you will silently edit the CSS instead.

---

## 10. Housekeeping

The earlier passport build (`index-passport.html`), the raw sample copy (`newspaper.html`), and the
20 passport-era crops have all been deleted. `assets/img/` now holds exactly the `np-*` files this
build uses — nothing in there is spare.

---

## 11. The link preview

What a guest sees when the URL is pasted into Messenger, Viber, WhatsApp or iMessage. All of it lives in
the `<head>`, and none of it is placeholder — the title and description are already the couple's.

| | Value |
|---|---|
| Title | Randy & Laurice — The Moment We Say “I Do” |
| Description | A special edition. You are invited. |
| Image | `assets/img/np-share.jpg`, 1200×630 — the bamboo-hut photograph, the same shot as the cover the link opens on |
| Icon | `favicon.ico` (16/32/48) and `apple-touch-icon.png` (180) — a cream ampersand on wine-black |

⚠️ **`og:image`, `og:url` and `twitter:image` are absolute URLs and have to stay that way.** A relative
`og:image` is silently dropped by every chat app — and since the card asks for `summary_large_image`,
that failure renders as an empty grey box rather than no image. It shipped that way once.

**If the domain ever changes**, five values need updating together: `canonical`, `og:url`, `og:image`,
`twitter:image`, and the `CNAME` file. Nothing computes them from the others.

⚠️ **Facebook and Messenger cache a preview the first time a link is posted.** Editing these tags will
not refresh what has already been cached — the URL has to be re-scraped through Facebook's Sharing
Debugger (`developers.facebook.com/tools/debug/`). Worth knowing before the invitation circulates.
