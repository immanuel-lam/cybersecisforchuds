# The Academic Exam Paper UI Style

A replication guide for the warm-paper, serif-editorial aesthetic.
Use this as a reference or paste directly into a Claude prompt.

---

## What This Style Is

An interface that looks like a beautifully typeset physical document — exam paper, academic journal, archival print. Warm off-white background, near-black ink, serif type for content, monospace for all UI chrome. No cards. No gradients. No glow. The page is the container; typography does all the work.

**Best for:** quizzes, study tools, reading apps, portfolios, documentation, anything where the content is the point.

---

## The Palette

```css
:root {
  --paper:      #F6F1E8;   /* warm parchment — the background */
  --paper-dark: #EDE7D9;   /* slightly deeper, for hover tints */
  --ink:        #1A1714;   /* near-black, warmer than #000 */
  --ink-mid:    #6B6560;   /* secondary text, labels */
  --ink-faint:  #B8B0A6;   /* placeholder, disabled, borders */
  --ink-ghost:  #DDD8D0;   /* very subtle dividers, ghost elements */
  --rule:       #E2DCD2;   /* 1px horizontal rules */
  --red:        #B91C1C;   /* error / incorrect */
  --red-bg:     #FEF2F2;
  --green:      #15643A;   /* success / correct */
  --green-bg:   #F0FDF4;
}
```

Never use pure `#FFFFFF` or `#000000`. The warmth of `#F6F1E8` and `#1A1714` is what makes it feel like paper and ink rather than a screen.

---

## The Fonts

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=IBM+Plex+Mono:wght@400;500;600&family=DM+Sans:opsz,wght@9..40,400;9..40,500&display=swap" rel="stylesheet">
```

| Role | Font | Usage |
|---|---|---|
| **Display / Content** | `Instrument Serif` | Headings, questions, main body copy |
| **Chrome / UI** | `IBM Plex Mono` | Labels, counters, buttons, metadata |
| **Body / Supporting** | `DM Sans` | Answer options, secondary paragraphs |

**Why this works:** The serif carries weight and authority. The mono adds technical precision. The sans keeps body copy readable without competing. Three registers, all harmonious.

---

## Typography Rules

```css
/* Display text — questions, headings */
font-family: 'Instrument Serif', serif;
font-size: 26–28px;
line-height: 1.42;
letter-spacing: -0.01em;   /* slight negative tracking on large serifs */

/* Chrome labels — ALL CAPS metadata */
font-family: 'IBM Plex Mono', monospace;
font-size: 10–11px;
font-weight: 600;
letter-spacing: 0.14–0.16em;
text-transform: uppercase;

/* Buttons */
font-family: 'IBM Plex Mono', monospace;
font-size: 11px;
font-weight: 600;
letter-spacing: 0.10em;
text-transform: uppercase;

/* Body / options */
font-family: 'DM Sans', sans-serif;
font-size: 15px;
line-height: 1.55;

/* Counters and numbers */
font-variant-numeric: tabular-nums;  /* stops numbers shifting width */
```

---

## Layout Structure

No centered cards. No `max-width` wrapper with a drop shadow. The layout is:

```
[sticky top chrome bar — 52px, border-bottom: 1px solid var(--rule)]
[main content — max-width: 820px, margin: 0 auto, padding: 56px 48px]
```

The chrome bar holds the title (left, mono small-caps) and a live counter (right, mono). It's sticky so users always know where they are.

```css
.chrome {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 48px;
  height: 52px;
  border-bottom: 1px solid var(--rule);
  position: sticky;
  top: 0;
  background: var(--paper);
  z-index: 10;
}
```

---

## The Ghost Number

The signature element of this style. A large, low-opacity number sits *behind* the content, anchoring it typographically.

```css
.ghost-number {
  position: absolute;
  top: -32px;
  left: -4px;
  font-family: 'Instrument Serif', serif;
  font-size: 130px;
  line-height: 1;
  color: var(--ink-ghost);   /* #DDD8D0 */
  user-select: none;
  pointer-events: none;
  z-index: 0;
  opacity: 0.5;
}

.content {
  position: relative;
  z-index: 1;
}
```

Works for: question numbers, section numbers, step indicators, page counts.

---

## Progress Bar

Not a thick rounded pill. A single 1px line:

```css
.progress-row {
  display: flex;
  align-items: center;
  gap: 18px;
  margin-bottom: 60px;
}

.progress-counter {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  font-weight: 600;
  color: var(--ink-mid);
  white-space: nowrap;
  letter-spacing: 0.06em;
}

.progress-track {
  flex: 1;
  height: 1px;
  background: var(--ink-ghost);
}

.progress-fill {
  height: 100%;
  background: var(--ink);
  transition: width 0.55s cubic-bezier(0.4, 0, 0.2, 1);
}
```

Display the counter as `"07 / 35"` — zero-padded, monospace, with spaces around the slash.

---

## Topic / Category Label

Not a pill badge. A small dot + small-caps mono label:

```css
.topic-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 22px;
}

.topic-pip {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  flex-shrink: 0;
  /* background: set per category */
}

.topic-label {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--ink-mid);
}
```

The dot gets a category color. The text stays `var(--ink-mid)`. The color lives in the dot, not in a glowing badge.

---

## List-Item Options (Buttons)

The answer options are styled as a bordered list — no cards, no rounded boxes:

```css
.options {
  border-top: 1px solid var(--rule);
}

.option {
  all: unset;
  display: flex;
  align-items: flex-start;
  gap: 18px;
  width: 100%;
  padding: 17px 4px 17px 10px;
  border-bottom: 1px solid var(--rule);
  position: relative;
  cursor: pointer;
}

/* The left-bar accent */
.option::before {
  content: '';
  position: absolute;
  left: 0; top: 0; bottom: 0;
  width: 3px;
  background: transparent;
  transition: background 0.18s;
}

/* Hover state */
.option:hover::before       { background: var(--ink); }
.option:hover               { background: rgba(26,23,20,0.03); }
.option:hover .opt-key      { background: var(--ink); color: var(--paper); border-color: var(--ink); }

/* Correct state */
.option[data-state="correct"]::before { background: var(--green); }
.option[data-state="correct"]         { background: var(--green-bg); }
.option[data-state="correct"] .opt-key { background: var(--green); border-color: var(--green); color: #fff; }

/* Wrong state */
.option[data-state="wrong"]::before   { background: var(--red); }
.option[data-state="wrong"]           { background: var(--red-bg); }
.option[data-state="wrong"] .opt-key  { background: var(--red); border-color: var(--red); color: #fff; }

/* Dimmed (other options after reveal) */
.option[data-state="dim"] { opacity: 0.4; cursor: default; }
```

The key element (A / B / C / D) is a small square with a border:

```css
.opt-key {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  font-weight: 600;
  width: 22px;
  height: 22px;
  min-width: 22px;
  border: 1.5px solid var(--ink-faint);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--ink-faint);
  transition: all 0.15s;
  flex-shrink: 0;
  margin-top: 1px;
}
```

On reveal: show `✓` for correct, `✗` for wrong.

---

## Buttons

Flat ink-black. No gradients, no shadows, no border-radius.

```css
.btn {
  all: unset;
  cursor: pointer;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.10em;
  text-transform: uppercase;
  background: var(--ink);
  color: var(--paper);
  padding: 12px 28px;
  white-space: nowrap;
  transition: opacity 0.15s;
}

.btn:hover { opacity: 0.72; }
```

For a secondary/outline variant:

```css
.btn-outline {
  background: transparent;
  color: var(--ink);
  border: 1.5px solid var(--ink);
}
.btn-outline:hover { background: var(--ink); color: var(--paper); }
```

---

## Feedback / Inline Status Bar

After an action, a feedback strip appears — anchored by a 2px top border, not a floating card:

```css
.feedback {
  margin-top: 32px;
  padding-top: 20px;
  border-top: 2px solid var(--ink);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  flex-wrap: wrap;
  animation: fadeUp 0.22s ease;
}

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}

.feedback-msg.ok  { color: var(--green); font-size: 14px; }
.feedback-msg.bad { color: var(--red);   font-size: 14px; }
```

---

## Results / Score Screen

The score is displayed as a single huge typographic number — not in a donut chart, not in a box:

```css
.results-score {
  font-family: 'Instrument Serif', serif;
  font-size: 104px;
  line-height: 0.9;
  color: var(--ink);
  letter-spacing: -0.02em;
}

.results-header {
  display: flex;
  align-items: flex-end;
  gap: 28px;
  padding-bottom: 36px;
  border-bottom: 2px solid var(--ink);  /* the big separator */
  margin-bottom: 48px;
}
```

Grade badge uses a bordered inline element, not a rounded pill:

```css
.grade-badge {
  display: inline-block;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  border: 1.5px solid currentColor;
  padding: 5px 12px;
}

.grade-badge.distinction { color: var(--green); }
.grade-badge.credit      { color: #1D4ED8; }
.grade-badge.pass        { color: #B45309; }
.grade-badge.fail        { color: var(--red); }
```

---

## Animations

Only three animations, used sparingly:

```css
/* Page/section entrance */
@keyframes slideIn {
  from { opacity: 0; transform: translateX(16px); }
  to   { opacity: 1; transform: translateX(0); }
}

/* Feedback bar appearance */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* Progress fill */
transition: width 0.55s cubic-bezier(0.4, 0, 0.2, 1);
```

All transitions: `0.15–0.28s`. Nothing slower unless it's a deliberate reveal.

---

## Paper Grain Texture

A subtle noise layer applied to `body` via an inline SVG data URI — no external image needed:

```css
body {
  background-color: var(--paper);
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)' opacity='0.03'/%3E%3C/svg%3E");
}
```

Opacity `0.03` is the sweet spot — visible up close, invisible at a glance. Go higher and it looks dirty.

---

## Mobile

```css
@media (max-width: 600px) {
  .chrome               { padding: 0 20px; }
  .main, .results       { padding: 36px 20px 72px; }
  .question-text        { font-size: 21px; }
  .ghost-number         { font-size: 90px; top: -20px; }
  .results-score        { font-size: 72px; }
}
```

The style holds well on mobile because it's layout-light — no complex grid to collapse.

---

## The Checklist

Before shipping, verify:

- [ ] Background is warm (`#F6F1E8`), not pure white
- [ ] No `border-radius` above `4px` anywhere except the topic dot
- [ ] No gradient on any button
- [ ] No `box-shadow` on floating cards (there are no floating cards)
- [ ] Display font is Instrument Serif, not Inter or Roboto
- [ ] All buttons are uppercase mono with `letter-spacing`
- [ ] Left-bar accent on list items (not a glow or thick ring)
- [ ] Ghost number is present and behind content (`z-index: 0`)
- [ ] Progress is a 1px line with a mono counter, not a thick pill
- [ ] State colors (correct/incorrect) use `var(--green)` / `var(--red)` — not neon
- [ ] Mobile breakpoint tested
