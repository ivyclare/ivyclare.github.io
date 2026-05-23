---
phase: 1
slug: skeleton-home-mvp
status: approved
shadcn_initialized: false
preset: none
created: 2026-05-19
---

# Phase 1 — UI Design Contract: Skeleton + Home MVP

> Visual and interaction contract for the entire site shell + the Home page. This contract is **shared with Phase 2 pages** (Publications, Talks, Writing, Code) — the same `style.css` powers them. Pick once, lock once.

**Revision 2026-05-19:** Restructured to Carlini-style layout after owner reviewed v1 implementation. Header is now a top bar (site title left, nav right, divider line below). Bio area is a two-column layout (bio text left, boxed profile card right). Typography switched to a **serif** system stack with **small-caps** for site title / nav / section headings. Single font weight (400) throughout — visual hierarchy comes from size, color, and small-caps, never from weight changes. Teal accent unchanged.

---

## Design System

| Property | Value |
|----------|-------|
| Tool | none — hand-written HTML + CSS |
| Preset | not applicable |
| Component library | none |
| Icon library | none — text-only contact links (no icon font, no SVG sprite) |
| Font | **Serif** system font stack: `"Iowan Old Style", "Palatino Linotype", "Book Antiqua", Palatino, "Hoefler Text", "Times New Roman", Georgia, serif` (zero external dependency) |
| Reference site | nicholas.carlini.com — visual layout, small-caps headings, profile-card aside |

**Why no design system / no library:** Hard constraint from PROJECT-BRIEF.md §3. The site must serve on GitHub Pages with zero configuration and no CDN dependencies. Plain HTML + a single `style.css` is the entire build.

---

## Site Shell (applies to all 5 pages, locked here for Phase 2)

### Layout

- **Page outer width: `1100px` max** on `.site-header` and `<main>` — wider than the old single-column 720px to accommodate the two-column bio layout.
- **Page padding:** `24px` horizontal on mobile, `40px` on desktop. Vertical: `32px` (header top) / `48px` (main top), `64px` bottom.
- **Centered:** both `.site-header` and `<main>` use `margin-inline: auto`.

### Site header — top bar (every page)

The site header is a **single horizontal bar** spanning the full content width with a divider line below.

- **Left:** `<a class="site-title" href="index.html">Ivoline Ngong</a>` — large (28px desktop / 24px mobile), serif, **small-caps**, letter-spacing 0.06em, color `--text-strong`, no underline. Clicking returns Home.
- **Right:** `<nav class="site-nav" aria-label="Site">` — 5 nav items in order **`Main`, `Papers`, `Talks`, `Code`, `Writing`**, no separator characters (generous flex gap instead: 32px desktop / 16px mobile). Each item rendered in 15px serif, **small-caps**, letter-spacing 0.08em.
- **Item color:** Active page (current view) in `--accent` (teal). Real links (other Phase 2 pages once they exist) also `--accent`, underline on hover. Phase 1 placeholders (Papers/Talks/Code/Writing) rendered as `<span data-todo="phase-2">` in `--text-muted` (gray) to signal they aren't yet clickable. After Phase 2 ships, swap placeholders to `<a>` in `--accent`.
- **Divider:** 1px `--border` horizontal line under the entire header.
- **Mobile (<640px):** site header stacks — title above, nav row below (still horizontal flex, generous gaps).

Nav label → file mapping:
- Main → `index.html`
- Papers → `publications.html`
- Talks → `talks.html`
- Code → `code.html`
- Writing → `writing.html`

### Skip link

A visually-hidden `<a href="#main">Skip to main content</a>` at the top of `<body>`, becomes visible on focus. Required for keyboard a11y.

### Footer

**None.** Carlini-style sites omit footers. No copyright notice, no "powered by" line.

---

## Home page structure (`index.html`)

DOM order top-to-bottom (this is the executor's contract):

1. **`<a class="skip-link">`** — visually hidden, visible on focus
2. **`<header class="site-header">`** — site title (left) + `<nav class="site-nav">` (right) + bottom divider
3. **`<main id="main">`**
   1. **`<div class="bio-layout">`** — two-column grid: bio text left, profile card right (single column on mobile / <900px)
      - **`<article class="bio-text">`** — Bio paragraph(s) extracted verbatim from wayback Home HTML. Multi-paragraph. Inline links (Joe Near, OpenMined, IBM Research mentors, etc.) preserved.
      - **`<aside class="profile-card">`** — boxed identity panel (see Profile Card below)
   2. **`<section id="news">`** — "What's New?" heading + **scrollable news container** (see Scrollable News below)
4. **`<body>` closes**

No `<h1>` element on Home — the `.site-title` (top-left of header) provides the document's primary identity. Section headings (`<h2>`) carry hierarchy from there.

---

## Scrollable News section

**This is the single most important interaction contract in Phase 1.** Owner explicitly clarified: the news section must be a fixed-height scrollable container, not a long inline dump of all ~2021–2025 entries.

### Behavior

- Section heading: `<h2>What's New?</h2>` (matches original copy).
- Below the heading, a `<div class="news-list">` container.
- **Container styling:**
  - `max-height: 380px` on desktop, `max-height: 320px` on mobile (< 640px)
  - `overflow-y: auto`
  - `padding-right: 8px` (so scrollbar doesn't crowd content)
  - `border: 1px solid var(--border)` and `border-radius: 6px`
  - `padding: 12px 16px`
  - Custom scrollbar styling (subtle, not the default chrome look) — see CSS Tokens below
- **News entry markup:** each entry is `<p class="news-entry"><span class="news-date">May 2025:</span> <span class="news-body">...</span></p>` so the date can be visually de-emphasized (smaller / muted color) and the body stays primary.
- **Order:** reverse chronological (newest at top), matching original.
- **Entries:** every entry from the wayback Home page, **extracted verbatim** including embedded paper-title links.

### Why scrollable

There are ~30+ news entries spanning 2021–2025. Inlining them all would make the bio + news together push the contact section below the fold by 3+ screens. A bounded scrollable container keeps the page scannable at a glance while preserving the entire history.

### Keyboard & a11y

- Container has `tabindex="0"` so it can receive keyboard focus.
- `aria-label="News updates, scrollable list"` for screen readers.
- Focus ring on the container uses `--accent` outline.

---

## Profile card (right column of bio layout)

A boxed identity panel that sits to the right of the bio text on desktop and stacks below the bio on mobile (<900px).

**Markup contract:**
```html
<aside class="profile-card" aria-label="Contact information">
  <img class="headshot" src="assets/profile_pic.png" alt="Portrait of Ivoline Ngong">
  <div class="identity">
    <p class="name">Ivoline Ngong</p>
    <p class="role">PhD Candidate, UVM</p>
    <p class="email"><a href="mailto:ivolinengong@gmail.com">ivolinengong [at] gmail [dot] com</a></p>
  </div>
  <p class="profile-links">
    <a href="https://github.com/ivyclare">GitHub</a><span class="sep">|</span>
    <a href="...">Google Scholar</a><span class="sep">|</span>
    <a href="...">LinkedIn</a><span class="sep">|</span>
    <a href="assets/Ivoline_Ngong_CV_August_2025.pdf">CV</a>
  </p>
</aside>
```

**Visual contract:**
- Card: light gray background (`--card-bg: #FAFAFA`), 1px `--border`, 8px border-radius, padding 24px, centered text.
- Photo: square aspect ratio (1:1), full card width (~272px inside 320px column on desktop), 6px border-radius (slightly rounded — NOT a circle), `object-fit: cover`, `object-position: center 25%`.
- Name: 17px, color `--text-strong`.
- Role: 17px, color `--text` (one line below name).
- Email: 17px, displayed as **`ivolinengong [at] gmail [dot] com`** with `href="mailto:ivolinengong@gmail.com"` (display text is obfuscated to deter naive scrapers; the actual link still works for humans clicking it). Styled as a normal teal underlined link.
- Profile links row: 5 links separated by `|` pipe characters with `--text-muted` color. Order: **GitHub | Google Scholar | Twitter | LinkedIn | CV** (matches the wayback contact set, just reorganized into the card format and pipe-separated).
- Card max-width on mobile: 360px, centered.

**Email obfuscation rationale:** Carlini uses the same `[at]` / `[dot]` pattern. Spam bots that scrape `mailto:` href values still get the address, but bots that scrape visible text get the obfuscated version. Minor friction for legitimate visitors (cannot one-click copy the visible address) is acceptable.

**Twitter included** to preserve content fidelity with the wayback source. The owner can drop it post-Phase-1 if they want by removing the `<a href="https://twitter.com/IvolineNgong">Twitter</a><span class="sep">|</span>` block from the profile card.

---

## Spacing Scale

Declared values (all multiples of 4 — grid-aligned):

| Token | Value | Usage |
|-------|-------|-------|
| `--space-0` | 0 | Reset |
| `--space-1` | 4px | Tight inline gaps |
| `--space-2` | 8px | Inline padding, link gap, scrollbar width |
| `--space-3` | 12px | News-entry vertical rhythm (intermediate step) |
| `--space-4` | 16px | Default paragraph spacing, news container padding |
| `--space-6` | 24px | Page edge padding (mobile), section margins |
| `--space-8` | 32px | Page edge padding (tablet), section breaks |
| `--space-10` | 40px | Page edge padding (desktop) (intermediate step) |
| `--space-12` | 48px | Top page padding |
| `--space-16` | 64px | Bottom page padding, major section breaks |

**Off-canonical-set tokens (`12px`, `40px`):** these are intentional intermediate steps that remain 4-aligned. `12px` provides comfortable news-entry rhythm (tighter than 16, looser than 8). `40px` is the desktop edge-padding sweet spot between 32 and 48. The grid-alignment invariant (multiples of 4) is preserved; the canonical set (4/8/16/24/32/48/64) is treated as a strong default with two declared intermediates.

Exceptions: none beyond the documented intermediates above.

---

## Typography

Serif system font stack used for every text element:

```css
font-family: "Iowan Old Style", "Palatino Linotype", "Book Antiqua", Palatino, "Hoefler Text", "Times New Roman", Georgia, serif;
```

Why serif: matches Carlini reference, signals academic personal site, renders well at body sizes on macOS (Iowan Old Style) and Windows (Palatino Linotype), with Georgia as the universal fallback.

**Type scale: 4 sizes, 1 weight.** Visual hierarchy comes from size + color + small-caps, not from weight changes. Body / news / profile-card paragraphs all share weight 400. Site title, nav, and section headings use small-caps + size to stand apart.

| Role | Size | Weight | Line Height | Letter spacing | Variant |
|------|------|--------|-------------|----------------|---------|
| Body / bio / news / profile-card / links | 17px desktop / 16px mobile (<640px) | 400 | 1.6 | normal | normal |
| Small / muted (news-date, profile-card sep, mobile nav) | 15px (14px on mobile) | 400 | 1.45 | normal | normal |
| Site nav links (`.site-nav`) | 15px desktop / 14px mobile | 400 | 1.4 | 0.08em | small-caps |
| H2 section heading (`What's New?`, `Publications`, etc.) | 22px | 400 | 1.3 | 0.05em | small-caps |
| Site title (`.site-title`) | 28px desktop / 24px mobile | 400 | 1.2 | 0.06em | small-caps |

**Distinct scale steps:** 4 (15, 17, 22, 28). Mobile variants (14, 16, 24) are responsive adaptations of the same role, not new scale steps.
**Distinct weights:** 1 (400 everywhere).

**Why one weight:** Carlini's reference uses one weight throughout; emphasis comes from small-caps for headings, color shifts (`--accent`, `--text-muted`), and size — not weight. Inline `<b>` and `<em>` tags inside bio/news still render bold/italic (the browser default for those elements), so emphasis within prose is preserved.

**No H1 element.** The `.site-title` in the top header carries the document's primary identity. H2 is the first heading level used in `<main>`. H3 is unused in Phase 1.

**No monospace** in Phase 1 (the wayback content has none). If Phase 2/3 introduces inline code, reuse the 15px Small role with `font-family: ui-monospace, "SF Mono", Menlo, Consolas, monospace`.

**Body text color:** `var(--text)`. Headings/site title: `var(--text-strong)`. Muted: `var(--text-muted)`. Links: `var(--accent)`.

---

## Color

| Role | Value | Usage |
|------|-------|-------|
| **Dominant (~80%)** — Background | `#FFFFFF` | Page background, news container fill |
| **Secondary (~15%)** — Body text | `#1F2937` (slate-800) | All body copy |
| **Muted (~3%)** — Tertiary text | `#6B7280` (slate-500) | News dates, role line, captions |
| **Accent (~2%)** — Teal | `#0F766E` (teal-700) | Links, active nav item, focus rings, top accent rule under header (optional 2px) |
| **Border** | `#E5E7EB` (slate-200) | News container border, hairline rules |
| **Focus ring** | `#0F766E` (same as `--accent`, 2px outline with 2px offset) | All focused interactive elements |

### Contrast verification (WCAG)

| Pair | Ratio | Standard |
|------|-------|----------|
| `#1F2937` body on `#FFFFFF` | 14.6 : 1 | AAA Normal (>= 7) ✓ |
| `#0F766E` accent on `#FFFFFF` | 5.5 : 1 | AA Normal (>= 4.5) ✓ |
| `#6B7280` muted on `#FFFFFF` | 4.7 : 1 | AA Normal (>= 4.5) ✓ |
| `#0F766E` focus on `#FFFFFF` | 5.5 : 1 | AA Non-text / UI (>= 3) ✓ |

The focus ring reuses `--accent` rather than a separate teal-500 — teal-500 (`#14B8A6`) was originally specified at 3.0:1 but actually measures ~2.49:1 on white (fails WCAG 2.1 1.4.11). Using `--accent` for the focus outline keeps the math correct, simplifies the token set, and the 2px outline + 2px offset already provides visual separation from underlined links.

All accent uses on white are accessible. Re-verify if accent value is changed.

### Accent reserved for

Explicit allowed list (so the accent stays scarce and meaningful):

1. Hyperlinks in body and nav (default color, underlined)
2. Active-page indicator in top nav
3. Section heading underline accent (optional — 2px wide teal rule under `<h2>` if used)
4. Focus rings (uses `--accent` / `#0F766E`)
5. CV link icon affordance (if any icon-like marker — otherwise skip)

**Never use accent for:** large background fills, button fills, hover state of every element, decorative dividers (use `--border` for those).

### CSS variables (single block at top of `style.css`)

```css
:root {
  --bg:           #FFFFFF;
  --text:         #1F2937;
  --text-strong:  #111827;
  --text-muted:   #6B7280;
  --accent:       #0F766E;
  --border:       #E5E7EB;
  --card-bg:      #FAFAFA;

  --space-1: 4px;  --space-2: 8px;  --space-3: 12px;
  --space-4: 16px; --space-6: 24px; --space-8: 32px;
  --space-10: 40px; --space-12: 48px; --space-16: 64px;

  --measure: 1100px;
  --news-max-h: 380px;
  --news-max-h-mobile: 320px;

  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
}
```

---

## Profile Photo (`assets/profile_pic.png`)

- Rendered as `<img class="headshot" alt="Portrait of Ivoline Ngong" loading="lazy" decoding="async">` inside the profile card.
- **Display size:** fills the inner width of the `.profile-card` (≈ 272px on desktop inside a 320px column, smaller on narrower viewports). Aspect ratio locked to `1 / 1` via `aspect-ratio: 1 / 1`.
- **Shape:** **6px border-radius** (slightly rounded square — NOT a full circle). Matches Carlini's reference card.
- **Object-fit:** `cover`, with `object-position: center 25%` to bias toward face if the source image is tall.
- **No drop shadow, no border on the image itself** — the surrounding card border + light gray background provide visual containment.

---

## Responsive Breakpoints

| Breakpoint | Width | Behavior |
|------------|-------|----------|
| Mobile | `< 640px` | Single column, photo stacked, body 16px, news max-h 320px, page padding 24px |
| Tablet | `≥ 640px` | Photo right-aligned next to H1, body 17px, news max-h 380px, page padding 32px |
| Desktop | `≥ 1024px` | Same as tablet (no special desktop variant — content stays at 720px max-width and is centered) |

Use only CSS media queries — no JS resize handlers.

---

## Interaction Details

### Links

- Default state: teal (`--accent`), `text-decoration: underline`, `text-decoration-thickness: 1px`, `text-underline-offset: 3px`.
- Hover: same color, `text-decoration-thickness: 2px`.
- Visited: same as default (no purple shift).
- Focus: 2px outline `--accent` with 2px offset.
- External links: no special indicator (Carlini-minimal). The URL itself is the affordance.

### Nav active state

- `<a aria-current="page">` styled in `--accent` color, no underline.
- All other nav links: dark-gray (`--text-strong`), underline on hover only.

### Scroll behavior

- News container scrolls internally. **Body does NOT have `scroll-behavior: smooth`** (Carlini-minimal — instant scroll is fine).
- News container custom scrollbar (WebKit + Firefox):

```css
.news-list { scrollbar-width: thin; scrollbar-color: var(--border) transparent; }
.news-list::-webkit-scrollbar { width: 8px; }
.news-list::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
.news-list::-webkit-scrollbar-thumb:hover { background: #D1D5DB; }
```

### Animation

**None.** No fades, no slide-ins, no hover transitions beyond `text-decoration-thickness`. The site loads, you read it. That's it.

### Reduced motion

Add a single `@media (prefers-reduced-motion: reduce)` rule that removes any transitions if a Phase 2 page adds them. Currently a no-op.

---

## Copywriting Contract

This site is content-driven (extracted verbatim from wayback). Copywriting decisions are limited to:

| Element | Copy |
|---------|------|
| Page `<title>` (Home) | `Ivoline Ngong` |
| Page `<title>` (Publications) | `Publications — Ivoline Ngong` |
| Page `<title>` (Talks) | `Talks — Ivoline Ngong` |
| Page `<title>` (Writing) | `Writing — Ivoline Ngong` |
| Page `<title>` (Code) | `Code — Ivoline Ngong` |
| `<meta name="description">` (Home) | `Ivoline Ngong — PhD candidate in Computer Science at the University of Vermont and research scientist at OpenMined, working at the intersection of privacy, security, safety, and AI.` |
| Skip link | `Skip to main content` |
| Photo alt text | `Portrait of Ivoline Ngong` |
| Section heading: news | `What's New?` (verbatim from original) |
| Section heading: bio | none — bio is the lede, no preceding heading |
| Nav labels | `Main` · `Papers` · `Talks` · `Code` · `Writing` (verbatim from original menu) |
| CV link label | `CV` |
| Email link label | `ivolinengong@gmail.com` (the email itself is the label) |
| Code page body (when built) | `My code lives on GitHub: <a href="https://github.com/ivyclare">github.com/ivyclare</a>` (decision: explicit link, not auto-redirect — keeps the site inert and gracefully handles users opening with JS disabled) |

**Voice:** Don't add filler. Don't write taglines. The bio in the wayback HTML is the voice. Use it untouched.

**No empty states / no error states** — this is a static personal site with no failure modes that need copy. Skip those rows from the template.

---

## Registry Safety

Not applicable. No shadcn, no third-party UI registry, no component library. Hand-written HTML and CSS only.

| Registry | Blocks Used | Safety Gate |
|----------|-------------|-------------|
| none | none | not applicable |

---

## File Layout for Phase 1

End of Phase 1, repo root should look like:

```
ivolinengong.com/  (repo root)
├── index.html               # Home page — implements this spec
├── style.css                # The single stylesheet — implements all tokens above
├── CNAME                    # Contains exactly: ivolinengong.com
├── assets/
│   ├── profile_pic.png      # Moved from repo root
│   └── Ivoline_Ngong_CV_August_2025.pdf  # Moved from repo root
├── PROJECT-BRIEF.md         # Already present
├── README.md                # Phase 3 deliverable, not Phase 1
├── reference/               # Source material — stays in repo for now (gitignored from prod later)
├── .planning/               # Planning docs
└── CLAUDE.md                # Already present
```

**Favicon:** Out of scope for Phase 1. Defer to Phase 3 polish (or add a single inline emoji-as-favicon then).

**index.html top-of-file scaffolding:**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Ivoline Ngong</title>
  <meta name="description" content="...">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <a class="skip-link" href="#main">Skip to main content</a>
  <nav aria-label="Site">...</nav>
  <main id="main">
    <header>...</header>
    <section id="about">...</section>
    <section id="news" aria-labelledby="news-heading">
      <h2 id="news-heading">What's New?</h2>
      <div class="news-list" tabindex="0" aria-label="News updates, scrollable list">
        ...entries...
      </div>
    </section>
  </main>
</body>
</html>
```

**No `<script>` tags in Phase 1.** Pure HTML + CSS.

---

## Open Items (deferred, not blocking)

- **Favicon design:** Phase 3.
- **OpenGraph metadata:** v2 (ENH-01).
- **Print stylesheet:** v2 (ENH-03).
- **Dark mode:** v2 (ENH-04).

---

## Checker Sign-Off

- [x] Dimension 1 Copywriting: PASS
- [x] Dimension 2 Visuals: PASS
- [x] Dimension 3 Color: PASS
- [x] Dimension 4 Typography: PASS
- [x] Dimension 5 Spacing: PASS
- [x] Dimension 6 Registry Safety: PASS (not applicable — no registry)

**Approval:** approved 2026-05-19 (gsd-ui-checker, 6/6 dimensions PASS after 1 revision pass)
