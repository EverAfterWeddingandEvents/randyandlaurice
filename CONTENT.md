# Content Checklist — Randy & Laurice

**Current build: `index.html` — "The Wedding Times" broadsheet, in English.**

**The names, dates, venue and story are now the real ones**, taken from `R_L_Wedding_Plan.xlsx`
(sheets `ENTOURAGE` and `STORY`). Anything the plan did not supply reads **`Pending`** on the page
rather than a fake value. Find what is still outstanding:

```bash
grep -n "Pending" index.html
```

### Still `Pending` — needs the couple

| Field | Where |
|---|---|
| RSVP deadline | front-page fact box, RSVP box, intro notice |
| Guest arrival time | programme, FAQ |
| Photographs / Send-off times | programme |
| GCash and bank details | "On Gifts" note |
| First met / first date | Notes from the Archive |
| Parking at the venue | FAQ |

**One correction to the plan.** The `ENTOURAGE` sheet has principal sponsor row 7 as
*Mary Jone G. Yap* under **Mr.** and *Asterio B. Yap* under **Ms.**, the reverse of every other pair.
Confirmed as a transcription slip and swapped on the page: Asterio under Gentlemen, Mary Jone under
Ladies. The spreadsheet itself still has it the other way round.

---

## 1. Do this first — the constants at the top of the `<script>`

```js
var WEDDING_DATE = '2026-10-30T14:30:00+08:00';   // countdown target, keep the +08:00 offset
var MAPS_URL     = 'https://maps.google.com/?q=Golden+Peak+Hotel+and+Suites+Cebu+City';
var IMG          = 'assets/img/';                 // leave alone
```

`WEDDING_DATE` is the **ceremony** time, 2:30 PM — that is what the countdown counts down to.

`MAPS_URL` drives the QR **and** the button beneath it. The two `View Directions ↗` links inside the
venue cards are separate `href`s in the markup and currently carry the same URL, because the plan
lists **one venue for the whole day**. If the ceremony moves to a church, those two `href`s and the
ceremony venue card have to change independently.

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

Also: `<title>`, the `og:` meta tags, `.nav-mark` (`R&L`), `.nf-mark` in the footer, and the
`R & L` script monogram in the closing message.

---

## 3. Names

All of these are now set from the `ENTOURAGE` sheet — 51 people.

| Field | Value |
|---|---|
| Bride / Groom | Michelle Laurice A. De Joya / Engr. Randy W. Odchigue |
| Bride's parents | Mr. Rey Arnel S. De Joya & Mrs. Eulogia A. De Joya |
| Groom's parents | Mr. Efrino G. Odchigue & Mrs. Eva W. Odchigue |
| Best Man / Maid of Honour | John Rey A. De Joya / Mellen Angelie W. Odchigue |
| Groomsmen & Bridesmaids | 5 rows each — add or remove `<tr>`s freely |
| Principal Sponsors | 11 rows |
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
| Guest arrival | ⏳ Pending | programme, FAQ |
| Photographs / Send-off | ⏳ Pending | programme |
| RSVP deadline | ⏳ Pending | front-page fact box, RSVP box, intro notice |
| City | ✅ Cebu City | mast bar, front-page caption, bylines, venue cards, closing block, intro dateline |
| Venue (both) | ✅ Golden Peak Hotel & Suites | front-page fact box, programme, venue cards |
| Address | ✅ Gorordo Avenue, corner North Escario Street, Cebu City, Philippines | venue cards |
| Hashtag | `#RandyAndLaurice` — derived from their names, not from the plan | front-page fact box, hashtag banner, FAQ |

The plan lists a single venue block after **both** the ceremony and the reception times, so both
venue cards carry Golden Peak. If the ceremony is actually at a church, that is the one detail to
correct.

**Timeline dates** from the `STORY` sheet feed the archive table: became a couple **June 20, 2024**,
proposal **September 29, 2025**, wedding **October 30, 2026** — which makes "Years Together" read
**2 years**.

---

## 5. Copy to make theirs

The prose is now the couple's own, lifted from the `STORY` sheet and split across the two article
blocks so neither column runs away with it.

- **"Brought Together by Chance"** — the first half of their story, from strangers through
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

Nothing was invented to fill a gap. Where the plan is silent, the page says `Pending`.

---

## 6. RSVP — ⚠️ NOT CONNECTED YET

The form is built and works end to end, **but nothing is being saved.** `RSVP_ENDPOINT` at the top of
the RSVP block in the inline `<script>` is an empty string. While it's empty the form validates and
shows its success state so it can be demoed, and logs a warning to the console — but replies go
nowhere. **Connect this before the invitation goes out.**

### Connecting it to Google Sheets (Apps Script)

1. Create a Google Sheet. Name a tab **`RSVPs`** and give it the header row:
   `Timestamp · Name · Attending · Seats · Message`
2. **Extensions → Apps Script**, and replace the contents with:

   ```js
   function doPost(e) {
     var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('RSVPs');
     sheet.appendRow([
       new Date(),
       e.parameter.name,
       e.parameter.attending,
       e.parameter.guests,
       e.parameter.message
     ]);
     return ContentService
       .createTextOutput(JSON.stringify({ ok: true }))
       .setMimeType(ContentService.MimeType.JSON);
   }
   ```

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
| `guests` | 1–4; sent as `0` automatically when they decline |
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
| `assets/photos/` | 31 originals, 2048px, **9.8 MB** | ❌ No — archive only |
| `assets/img/` | web-sized crops | ✅ Yes |

Newspaper set (`np-*`, ~2.5 MB total, 33 files):

| File | Where | Size |
|---|---|---|
| `np-cover.jpg` | Photo on the rolled intro paper | 760×440 |
| `np-share.jpg` | Link-preview card (`og:image`) | 1200×630 |
| `np-hero.jpg` | Front-page plate, **frame 1** | 1440×810 |
| `np-hero2–hero4.jpg` | Front-page plate, frames 2–4 | 1440×810 |
| `np-c1–c8.jpg` | Carousel, 16:9 | 900×506 |
| `np-s1–s10.jpg` | First photo strip | 560×400 landscape / 280×400 portrait |
| `np-r1–r10.jpg` | Second strip (reversed) | same |

The `type` (`landscape` / `portrait`) in the `PHOTOS` and `PHOTOS_REVERSE` arrays must match the
crop's actual shape — it sets the item's width. A portrait file in a landscape slot will stretch.

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

**All 31 originals are now spoken for**, so a fifth hero frame means reusing a photo that already appears
somewhere else on the page. `np-hero.jpg` and `np-c5.jpg` are already the same photograph — that predates
this build, but it is the kind of repeat worth avoiding if more crops get made.

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
| Title | Randy & Laurice — The Wedding Times |
| Description | A special edition. You are invited. |
| Image | `assets/img/np-share.jpg`, 1200×630 — the picnic photograph, so the card matches the cover the link opens on |
| Icon | `favicon.ico` (16/32/48) and `apple-touch-icon.png` (180) — a cream ampersand on wine-black |

⚠️ **`og:image`, `og:url` and `twitter:image` are absolute URLs and have to stay that way.** A relative
`og:image` is silently dropped by every chat app — and since the card asks for `summary_large_image`,
that failure renders as an empty grey box rather than no image. It shipped that way once.

**If the domain ever changes**, five values need updating together: `canonical`, `og:url`, `og:image`,
`twitter:image`, and the `CNAME` file. Nothing computes them from the others.

⚠️ **Facebook and Messenger cache a preview the first time a link is posted.** Editing these tags will
not refresh what has already been cached — the URL has to be re-scraped through Facebook's Sharing
Debugger (`developers.facebook.com/tools/debug/`). Worth knowing before the invitation circulates.
