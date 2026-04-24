# Dates & Rates — Refinements Handoff

**Staging URL:** `https://morashastg1.wpenginepowered.com/the-morasha-experience/dates-fees/`
**Refined mockup:** `morasha-dates-rates.html`

This is a diff between the current staging implementation and the refined design. Nav, footer, fonts, colors, and all copy are unchanged. The work is confined to four sections: hero, dates grid, tuition section, cancellation section.

---

## Summary of changes

1. **Hero** — shrink, rename h1, and remove the dates grid from inside it
2. **Dates section** — becomes its own section below the hero; redesigned grid with uniform hairline borders; Full Summer highlighted
3. **Tuition section** — new background color, cleaner table styling, swapped order of "Tuition includes" and Alufim footnote with visual emphasis flipped
4. **Cancellation section** — de-emphasized: smaller heading, background swapped, itemized list, dates bold / penalties muted

---

## 1. Hero (`.dp__header`)

### Current
- Background `#142b15` (correct, keep)
- Contains h1 "2026 Camp Morasha Dates" **and** the entire dates grid nested inside
- h1 font-size 56px desktop
- Padding 46px top / 4rem bottom

### Change to
- **Remove the dates grid from inside this section** (move it to its own section — see #2)
- **Change h1 text** from "2026 Camp Morasha Dates" → **"Dates & Rates"**
- Shrink the hero — it no longer needs to fit a grid
- Vertically center the h1 (currently the title sits wherever the flow puts it; we want it centered in the hero box)

### HTML diff

```html
<!-- BEFORE -->
<section class="dp__header">
  <div class="dp__header__inner container">
    <h1 class="dp__header__title">2026 Camp Morasha Dates</h1>
    <div class="dp__dates-grid">
      <!-- 6 date cards nested here -->
    </div>
  </div>
</section>

<!-- AFTER -->
<section class="dp__header">
  <div class="dp__header__inner container">
    <h1 class="dp__header__title">Dates &amp; Rates</h1>
  </div>
</section>
```

### CSS diff

```css
/* In .dp__header add flex-centering and a min-height, clear the padding-bottom */
.dp__header {
  display: flex;
  align-items: center;
  min-height: 320px;
  padding-top: 148px;   /* clears fixed two-tier nav */
  padding-bottom: 0;
}
.dp__header__inner { width: 100%; }

/* Remove the big bottom margin on the title that was leaving room for the dates grid */
.dp__header__title { margin-bottom: 0; }

@media (max-width: 1024px) {
  .dp__header { padding-top: 128px; min-height: 280px; }
}
@media (max-width: 640px) {
  .dp__header { padding-top: 104px; min-height: 220px; }
  .dp__header__title { font-size: 36px; }
}
```

---

## 2. Dates section (was nested inside `.dp__header`, now its own section)

### Current
- Nested inside the dark-green hero
- 3-column grid of white cards, centered text
- Label: light-green (`#9BBA91`) condensed bold uppercase, 20–24px
- Date: Roboto medium 24–36px in dark-green
- All cards identical (no highlight for Full Summer)

### Change to
- **Move out of the hero, make its own section** on a white background below
- Keep the 3-column grid, but:
  - **Left-align** label and date (not centered)
  - Label stays condensed uppercase but smaller (12px) and in **primary green** `#46764B` (not light green)
  - Date in **serif** (Kefa), 22–26px, dark green `#142B15` (promotes the date to the visual focus)
  - **Highlight the Full Summer card** with a light-green (`#DAE4D0`) background
  - **Uniform hairline borders** around every cell (the staging uses `gap`-based dividers which creates uneven corner treatment)
- Add a small section head above the grid: eyebrow "Camp Morasha" + h2 "2026 Dates"

### HTML diff

```html
<!-- BEFORE: grid was inside .dp__header with no section head -->

<!-- AFTER: new top-level section -->
<section class="dp__dates">
  <div class="dp__dates__inner container">
    <div class="dp__dates__head">
      <span class="dp__dates__eyebrow">Camp Morasha</span>
      <h2 class="dp__dates__title">2026 Dates</h2>
    </div>
    <div class="dp__dates-grid">
      <div class="dp__date-card dp__date-card--feature">
        <span class="dp__date-card__label">Full Summer</span>
        <span class="dp__date-card__date">June 30 – August 18</span>
      </div>
      <div class="dp__date-card">
        <span class="dp__date-card__label">First Session</span>
        <span class="dp__date-card__date">June 30 – July 28</span>
      </div>
      <!-- Second Session, Staff Orientation, Visiting Day, Morasha Mania in same pattern -->
    </div>
  </div>
</section>
```

### CSS diff

```css
.dp__dates {
  background: #fff;
  padding: 80px 0;
}
.dp__dates__inner { max-width: 1100px; margin: 0 auto; padding: 0 48px; }
.dp__dates__head { margin-bottom: 48px; }
.dp__dates__eyebrow {
  display: block;
  margin-bottom: 14px;
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 700;
  font-size: 13px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #46764B;
}
.dp__dates__title {
  font-family: "Kefa", Georgia, serif;
  font-weight: 700;
  font-size: clamp(32px, 3.5vw, 44px);
  color: #142B15;
  letter-spacing: -0.015em;
}

/* REPLACE the existing .dp__dates-grid and .dp__date-card styles */
.dp__dates-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0;                                  /* was 1.25rem gap */
  border-top: 1px solid rgba(20,43,21,0.12);
  border-left: 1px solid rgba(20,43,21,0.12);
}
.dp__date-card {
  background: #fff;
  padding: 32px 28px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;                 /* was: align-items: center (centered) */
  text-align: left;                        /* was: text-align: center */
  border-right: 1px solid rgba(20,43,21,0.12);
  border-bottom: 1px solid rgba(20,43,21,0.12);
  border-radius: 0;
}
.dp__date-card--feature {                  /* NEW modifier */
  background: #DAE4D0;
}
.dp__date-card__label {
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 700;
  font-size: 12px;                         /* was 20–24px */
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #46764B;                          /* was #9BBA91 */
  margin-bottom: 12px;
}
.dp__date-card__date {
  font-family: "Kefa", Georgia, serif;     /* was Roboto */
  font-weight: 700;                        /* was 500 */
  font-size: clamp(22px, 1.8vw, 26px);     /* was 24–36px */
  line-height: 1.15;
  letter-spacing: -0.01em;
  color: #142B15;                          /* was #2A522E */
}

@media (max-width: 1024px) {
  .dp__dates-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 640px) {
  .dp__dates-grid { grid-template-columns: 1fr; }
}
```

**Content fix while you're there:** "STaff Orientation" → "Staff Orientation" (capitalization on the staging page).

---

## 3. Tuition section (`.dp__tuition`)

### Current
- White background
- h2 "2026 Camp Morasha Tuition" — 56px serif
- Table: dark-green header row, **alternating row backgrounds** (odd `#DAE4D0`, even `#9BBA91`)
- Footnote paragraph (Alufim) — 18–20px Roboto, grey, centered, max-width 1060px
- "Tuition includes" paragraph below — 18–20px Roboto, charcoal, left-aligned

### Change to
- **Background** → lightest green `#DAE4D0`
- **h2 text** → "2026 Rates" (shorter), with a small condensed "Tuition" eyebrow above it
- **Table:** dark-green header row stays. **Drop the alternating row colors** — rows on white with hairline dividers. Prices right-aligned in serif. Grade column in serif.
- **Swap order and treatment of the two paragraphs below the table:**
  - Move "Tuition includes" **up** to sit immediately below the table, inside a white box with a small green "TUITION INCLUDES" eyebrow label
  - Move the Alufim footnote **down**, rendered as muted grey small sans-serif (fine print)

### HTML diff

```html
<!-- BEFORE -->
<section class="dp__tuition">
  <div class="dp__tuition__inner container">
    <h2 class="dp__tuition__title">2026 Camp Morasha Tuition</h2>
    <div class="dp__tuition__table-wrap">
      <table class="dp__tuition__table">…</table>
    </div>
    <p class="dp__tuition__footnote">*Alufim/fot (entering 10th grade)…</p>
    <p class="dp__tuition__included">Tuition includes transportation…</p>
  </div>
</section>

<!-- AFTER -->
<section class="dp__tuition">
  <div class="dp__tuition__inner container">
    <div class="dp__tuition__head">
      <span class="dp__tuition__eyebrow">Tuition</span>
      <h2 class="dp__tuition__title">2026 Rates</h2>
    </div>
    <div class="dp__tuition__table-wrap">
      <table class="dp__tuition__table">…</table>
    </div>
    <div class="dp__tuition__included">
      <strong>Tuition includes</strong>
      Transportation to camp, laundry, accident and secondary care, all camp trips, camp t-shirt, and end-of-summer divisional apparel (for Full Summer / Second Session campers). Tuition does not include canteen or counselor tips.
    </div>
    <p class="dp__tuition__footnote"><sup>*</sup> <strong>Alufim/fot</strong> (entering 10th grade): A $750 trip fee is added to partially cover the costs associated with the FimFotWest eight-day trip during the First Session. The six-day Road Trip during the Second Session is included in the listed tuition.</p>
  </div>
</section>
```

### CSS diff

```css
.dp__tuition {
  background: #DAE4D0;                     /* was white */
  padding: 80px 0;
}
.dp__tuition__inner { max-width: 1100px; margin: 0 auto; padding: 0 48px; }

/* New section head with eyebrow */
.dp__tuition__head { margin-bottom: 48px; }
.dp__tuition__eyebrow {
  display: block;
  margin-bottom: 14px;
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 700;
  font-size: 13px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #46764B;
}
.dp__tuition__title {
  font-family: "Kefa", Georgia, serif;
  font-weight: 700;
  font-size: clamp(32px, 3.5vw, 44px);    /* was 56px */
  color: #142B15;
  margin-bottom: 0;
  letter-spacing: -0.015em;
}

/* Table: drop the pastel row alternation, right-align prices, serif display numbers */
.dp__tuition__table { background: #fff; border-collapse: collapse; }
.dp__tuition__table thead tr { background: #142B15; }  /* was #2A522E — slightly darker for depth */
.dp__tuition__table thead th {
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 700;
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #fff;
  padding: 20px 24px;
  text-align: left;
}
.dp__tuition__table thead th:not(:first-child) { text-align: right; }
.dp__tuition__table tbody tr:nth-child(odd),
.dp__tuition__table tbody tr:nth-child(even) {
  background: #fff;                         /* kill the pastel alternation */
}
.dp__tuition__table tbody td {
  padding: 28px 24px;
  font-family: "Kefa", Georgia, serif;
  font-weight: 700;
  color: #142B15;
  vertical-align: middle;
  border-bottom: 1px solid rgba(20,43,21,0.1);
}
.dp__tuition__table tbody tr:last-child td { border-bottom: none; }
.dp__tuition__table td:first-child {       /* Grade column */
  font-size: 18px;
  width: 180px;
}
.dp__tuition__table td:not(:first-child) { /* Price columns */
  text-align: right;
  font-size: clamp(22px, 2vw, 28px);
  letter-spacing: -0.015em;
}

/* "Tuition includes" — now prominent, white box, green eyebrow */
.dp__tuition__included {
  background: #fff;
  padding: 24px 28px;
  margin-bottom: 20px;                      /* above, not below the footnote */
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-size: 16px;
  line-height: 1.65;
  color: #222;
  text-align: left;                         /* was center-ish via max-width auto margin */
}
.dp__tuition__included strong {
  display: block;
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 700;
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #46764B;
  margin-bottom: 8px;
}

/* Alufim footnote — now muted fine print */
.dp__tuition__footnote {
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-size: 14px;                          /* was 18–20px */
  color: #828282;
  line-height: 1.6;
  text-align: left;                         /* was centered */
  max-width: none;                          /* was 1060px centered */
  padding: 0 4px;
  margin: 0;
}
.dp__tuition__footnote sup { color: #46764B; font-weight: 700; }
.dp__tuition__footnote strong { color: #142B15; font-weight: 700; }
```

**Note:** the existing `.dp__tuition__table-wrap { overflow-x: auto }` rule should stay for mobile scroll.

---

## 4. Cancellation section (`.dp__cancel`)

### Current
- Background lightest green `#DAE4D0`
- h2 "Camper Refund Policy" — 28–36px Roboto medium, **centered**
- 2-column grid with cancellation rules as lines of plain text separated by `<br />`
- Small italic centered note at the bottom
- Feels like another hero-scale section competing with the pricing info above

### Change to
- **Background** → white (makes it clearly secondary to the lightest-green Tuition section above)
- **Dramatically de-emphasize the heading** — 20px tight serif, left-aligned, with small eyebrow "Refund policy" above. It's fine print, not a banner.
- **2-column head** — title on the left, the intro note becomes a paragraph on the right (not a center-aligned italic at the bottom)
- **Replace the 2-column text blobs** with an **itemized list** of hairline-divided rows: each row is `[deadline]   [penalty]`. Deadline bold in dark green on the left; penalty plain-weight grey on the right — the eye should go to the deadline, not the dollar amount.
- Keep the full-refund line visually distinct (red) without shouting

### HTML diff

```html
<!-- BEFORE -->
<section class="dp__cancel">
  <div class="dp__cancel__inner container">
    <h2 class="dp__cancel__title">Camper Refund Policy</h2>
    <div class="dp__cancel__columns">
      <div class="dp__cancel__col">Non-Accepted Campers: $1,500 Application Fee Returned<br />
Cancellations before October 1: $250 Penalty<br />
Cancellations after October 1: $1,500 Penalty</div>
      <div class="dp__cancel__col">Cancellations after December 1: $2,500 Penalty<br />
Cancellations after February 1: $3,500 Penalty<br />
Cancellations after April 1: No Refund</div>
    </div>
    <p class="dp__cancel__note">These cancellation fees are applicable to both One Session and Full Summer Campers</p>
  </div>
</section>

<!-- AFTER -->
<section class="dp__cancel">
  <div class="dp__cancel__inner container">
    <div class="dp__cancel__head">
      <div>
        <span class="dp__cancel__eyebrow">Refund policy</span>
        <h2 class="dp__cancel__title">Cancellation schedule</h2>
      </div>
      <p class="dp__cancel__note">Cancellation fees apply to both One Session and Full Summer campers. Full application fees are returned to non-accepted campers.</p>
    </div>
    <div class="dp__cancel__list">
      <div class="dp__cancel__row">
        <div class="dp__cancel__when">Non-accepted campers</div>
        <div class="dp__cancel__penalty dp__cancel__penalty--none">$1,500 Application fee returned</div>
      </div>
      <div class="dp__cancel__row">
        <div class="dp__cancel__when">Cancellations before October 1</div>
        <div class="dp__cancel__penalty">$250 penalty</div>
      </div>
      <div class="dp__cancel__row">
        <div class="dp__cancel__when">Cancellations after October 1</div>
        <div class="dp__cancel__penalty">$1,500 penalty</div>
      </div>
      <div class="dp__cancel__row">
        <div class="dp__cancel__when">Cancellations after December 1</div>
        <div class="dp__cancel__penalty">$2,500 penalty</div>
      </div>
      <div class="dp__cancel__row">
        <div class="dp__cancel__when">Cancellations after February 1</div>
        <div class="dp__cancel__penalty">$3,500 penalty</div>
      </div>
      <div class="dp__cancel__row">
        <div class="dp__cancel__when">Cancellations after April 1</div>
        <div class="dp__cancel__penalty dp__cancel__penalty--full">No refund</div>
      </div>
    </div>
  </div>
</section>
```

### CSS diff

```css
.dp__cancel {
  background: #fff;                         /* was #DAE4D0 */
  padding: 56px 0 72px;                     /* was 60px / 50px — tighter */
}
.dp__cancel__inner { max-width: 1100px; margin: 0 auto; padding: 0 48px; }

.dp__cancel__head {
  display: grid;
  grid-template-columns: 280px 1fr;         /* was: single-col, centered content */
  gap: 56px;
  align-items: start;
  margin-bottom: 24px;
}
.dp__cancel__eyebrow {
  display: block;
  margin-bottom: 10px;
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 700;
  font-size: 11px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #46764B;
}
.dp__cancel__title {
  font-family: "Kefa", Georgia, serif;      /* was Roboto medium */
  font-weight: 700;
  font-size: 20px;                          /* was 28–36px */
  line-height: 1.2;
  color: #142B15;
  text-align: left;                         /* was centered */
  margin-bottom: 0;
  letter-spacing: -0.005em;
}
.dp__cancel__note {
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-size: 13px;                          /* was 14px italic */
  font-style: normal;
  color: #828282;
  line-height: 1.6;
  text-align: left;                         /* was centered */
  max-width: 56ch;
}

/* REPLACE .dp__cancel__columns with the new itemized list */
.dp__cancel__list {
  border-top: 1px solid rgba(20,43,21,0.1);
}
.dp__cancel__row {
  display: grid;
  grid-template-columns: 340px 1fr;
  gap: 24px;
  padding: 12px 0;
  border-bottom: 1px solid rgba(20,43,21,0.08);
  align-items: baseline;
}
.dp__cancel__when {                         /* LEFT — emphasized */
  font-family: "Roboto", sans-serif;
  font-weight: 700;
  font-size: 14px;
  color: #142B15;
}
.dp__cancel__penalty {                      /* RIGHT — muted */
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-size: 14px;
  color: #828282;
}
.dp__cancel__penalty--none {
  color: #828282;
  font-style: italic;
}
.dp__cancel__penalty--full {
  color: #a3341f;                           /* muted red for "No refund" */
}

@media (max-width: 1024px) {
  .dp__cancel__head { grid-template-columns: 1fr; gap: 16px; margin-bottom: 32px; }
  .dp__cancel__row { grid-template-columns: 1fr; gap: 6px; padding: 10px 0; }
}
```

---

## Order of sections on the page

Stays the same sequence, but the Dates grid now lives in its own section instead of nested in the hero:

```
Hero (dp__header)       — dark green, h1 "Dates & Rates" only
Dates (dp__dates)       — white, 3-col grid, Full Summer featured   ← new section
Tuition (dp__tuition)   — lightest green, table + "Tuition includes" box + Alufim footnote
Cancellation (dp__cancel) — white, de-emphasized list
```

---

## Quick QA checklist

- [ ] "STaff Orientation" → "Staff Orientation" (typo fix on the staging page)
- [ ] h1 text changed to "Dates & Rates"
- [ ] Dates grid moved out of hero into its own section
- [ ] Full Summer card has light-green fill
- [ ] Tuition section background is lightest green
- [ ] Table rows are white (no pastel alternation)
- [ ] "Tuition includes" is in a white box ABOVE the Alufim footnote
- [ ] Alufim footnote is small grey fine print
- [ ] Cancellation section is on white, title is small serif, rows are hairline-divided
- [ ] Deadlines bold / penalties plain grey
