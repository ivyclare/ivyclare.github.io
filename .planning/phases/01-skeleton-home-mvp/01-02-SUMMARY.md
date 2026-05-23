---
phase: 01-skeleton-home-mvp
plan: 02
subsystem: static-site/style
status: complete
tags:
  - static-site
  - css
  - design-tokens
  - shared-stylesheet
requirements_completed:
  - DES-01
  - DES-02
  - DES-03
  - DES-05
  - DES-06
  - DES-07
  - INFRA-04
  - INFRA-05
dependency_graph:
  requires:
    - 01-UI-SPEC.md (design contract)
  provides:
    - style.css (single shared stylesheet for Phase 1 and Phase 2)
  affects:
    - 01-03 (index.html will consume these selectors)
    - Phase 2 pages (publications, talks, code, writing — same stylesheet)
tech-stack:
  added:
    - hand-written CSS (no preprocessor, no PostCSS, no build step)
  patterns:
    - CSS custom properties for tokens
    - mobile-first responsive design with two media queries (max-width:639px, min-width:1024px)
    - object-fit / object-position for headshot face crop
    - native CSS scrollbar styling (scrollbar-width + ::-webkit-scrollbar)
    - prefers-reduced-motion guard
key-files:
  created:
    - style.css
  modified: []
decisions:
  - middot-rendering: HTML-inline (Plan 03 will write plain `·` between nav/contact-strip items) — no CSS `::after` content rule. Keeps CSS simpler and means the separator is selectable/copyable text, which is friendlier for keyboard users.
  - optional-h2-underline: SKIPPED. The UI-SPEC §Accent reserved-for list flags this as optional. I chose the cleaner unbordered H2 because (a) the accent count is already healthy with links + active-nav + focus rings, (b) underlining the H2 visually fights with the news-container border below it, and (c) Carlini-style reference is restrained — adding the accent rule would be the only decorative touch in the whole sheet.
  - focus-ring-color: `--accent` (#0F766E, 5.5:1 on white). The UI-SPEC verified this passes WCAG 2.1 1.4.11 at 3:1 minimum and noted the originally-suggested teal-500 actually fails.
  - scrollbar-hover-color: `#D1D5DB` (slate-300) used verbatim from UI-SPEC §Scroll behavior — this is one literal value outside the token set, kept inline as a CSS-internal nuance rather than promoted to `:root`.
metrics:
  duration: ~5 minutes
  completed: 2026-05-19
  tasks_completed: 2
  files_created: 1
  files_modified: 0
  line_count: 323
---

# Phase 1 Plan 01-02: Shared style.css Summary

Single hand-written stylesheet (`style.css`, 323 lines) implementing every token, layout rule, and interaction contract from 01-UI-SPEC.md. Powers Home (Plan 03) and all four Phase 2 pages without modification.

## What was built

`/Users/ivolinengong/Documents/My_Website/style.css` — 323 lines, comment-organized into 14 sections matching the UI-SPEC structure.

### Sections present (in file order)

| Line | Section |
|------|---------|
| 1    | File header banner |
| 6    | `/* Tokens */` — `:root` block with all custom properties |
| 33   | `/* Reset & base */` — minimal box-sizing reset, no third-party reset |
| 48   | `/* Typography (body baseline) */` — system font stack, body 17px line-height 1.6, H1/H2 weight 700 + letter-spacing |
| 74   | `/* Mobile body size */` — `@media (max-width: 639px)` body → 16px |
| 81   | `/* Layout — page shell */` — `<main>` max-width 720px centered, padding 48/32/64 with mobile 24 / desktop 40 horizontal overrides |
| 102  | `/* Skip link */` — visually hidden default, visible on `:focus` with `--accent` bg |
| 126  | `/* Site nav */` — `nav[aria-label="Site"]` inactive links `--text-strong` no-underline (underline on hover), `[aria-current="page"]` in `--accent` |
| 149  | `/* Header & profile photo */` — flex row tablet+, column-reverse mobile, `.headshot` 120×120 (96×96 mobile), `border-radius: 50%`, `object-position: center 25%`, `header h1` 32px (26px mobile), `.role` and `.contact-strip` 14px muted |
| 207  | `/* Sections */` — `section { margin-block: var(--space-8); }`, H2 22px |
| 217  | `/* Bio paragraphs */` — `section#about p` 17px / 16px mobile, line-height 1.6 |
| 230  | `/* News scrollable container */` — `.news-list` max-height 380/320, `overflow-y: auto`, 1px `--border`, 6px radius, custom Firefox + WebKit scrollbar, `:focus` outline `--accent` 2px / 2px offset; `.news-entry` / `.news-date` / `.news-body` markup styling |
| 294  | `/* Links */` — global `a` teal underlined 1px / 2px on hover, no visited shift, `:focus-visible` 2px outline `--accent` |
| 315  | `/* Reduced motion */` — `@media (prefers-reduced-motion: reduce)` no-op guard |

## Tokens implemented (all literal UI-SPEC values)

```
--bg #FFFFFF    --text #1F2937    --text-strong #111827
--text-muted #6B7280    --accent #0F766E    --border #E5E7EB
--space-1 4   --space-2 8    --space-3 12    --space-4 16
--space-6 24  --space-8 32   --space-10 40   --space-12 48   --space-16 64
--measure 720px    --news-max-h 380px    --news-max-h-mobile 320px
--radius-sm 4px    --radius-md 6px
```

Every value copied verbatim. No new tokens introduced. No `--space-5`, `--space-7`, `--space-9`, `--space-11`, `--space-13`, `--space-14`, `--space-15` declared (UI-SPEC scale skips these intentionally).

## Selector contract for Plan 03

Plan 03 (index.html) can hard-code these selectors knowing they exist in style.css:

| Selector | Purpose |
|----------|---------|
| `.skip-link` | Top-of-body a11y link |
| `nav[aria-label="Site"]` + `nav a` + `[aria-current="page"]` | Top nav |
| `header`, `.headshot`, `header h1`, `header .role`, `header .contact-strip` | Header block |
| `section#about p` | Bio paragraphs |
| `section#news h2` (matches `section h2` rule) | News heading |
| `.news-list` + `.news-entry` + `.news-date` + `.news-body` | Scrollable news |
| `a` (global) | All hyperlinks |

## Verification gates — all passed

| Gate | Required | Actual | Status |
|------|----------|--------|--------|
| `wc -l style.css` | ≥ 120 | 323 | PASS |
| `grep -c 'var(--accent)' style.css` | ≥ 3 | 6 | PASS |
| `grep -c -- '--space-' style.css` | ≥ 9 | 29 | PASS |
| `! grep -q '@import' style.css` | absent | absent | PASS |
| `! grep -qE 'https?://[^/]*\.(googleapis\|gstatic\|jsdelivr\|unpkg\|cdn)'` | absent | absent | PASS |
| `grep -q 'overflow-y:[[:space:]]*auto'` | present | present | PASS |
| `grep -q 'border-radius:[[:space:]]*50%'` | present | present | PASS |
| `grep -q -- '--accent:[[:space:]]*#0F766E'` | present | present | PASS |
| `grep -q -- '--text:[[:space:]]*#1F2937'` | present | present | PASS |
| `grep -q -- '--bg:[[:space:]]*#FFFFFF'` | present | present | PASS |
| `grep -q -- '--measure:[[:space:]]*720px'` | present | present | PASS |
| `grep -q 'BlinkMacSystemFont'` | present | present | PASS |
| `grep -q 'prefers-reduced-motion'` | present | present | PASS |
| Brace balance (open vs close) | equal | 56 = 56 | PASS |
| Encoding | ASCII | ASCII | PASS |

## Acceptance criteria — all met

### Task 1
- [x] style.css exists at repo root
- [x] `:root` declares `--accent: #0F766E`, `--text: #1F2937`, `--bg: #FFFFFF`, `--measure: 720px`, all spacing tokens
- [x] `body` includes `BlinkMacSystemFont` font stack
- [x] No `@import`
- [x] No `fonts.googleapis.com` / `fonts.gstatic.com` / CDN refs
- [x] Plain CSS — no SCSS, no nesting, no `$variable`

### Task 2
- [x] `.news-list` with `overflow-y: auto` and `max-height` (380px default, 320px mobile via `@media (max-width: 639px)`)
- [x] `.headshot` with `border-radius: 50%`
- [x] ≥ 3 references to `var(--accent)` (actual: 6 — links, link-hover-inherits-default, visited, focus-visible, skip-link, nav active, news focus)
- [x] `@media (prefers-reduced-motion: reduce)` present
- [x] ≥ 120 lines (323)
- [x] No `@import`, no external font URLs

## Plan-level success criteria — all met

- [x] style.css at repo root, 323 lines
- [x] All tokens declared in `:root` per UI-SPEC
- [x] Site-shell rules: `body`, `main`, `nav`, `.skip-link`, global links, reduced-motion
- [x] Home-specific selectors: `.headshot`, `header h1`, `header .role`, `header .contact-strip`, `section#about`, `section h2`, `.news-list`, `.news-entry`, `.news-date`
- [x] No `@import`, no external font URL, no CDN
- [x] System font stack in `body`
- [x] Mobile (`max-width: 639px`) reduces body→16px, page padding→24px (`--space-6`), headshot→96×96, news max-h→320px, H1→26px
- [x] Desktop (`min-width: 1024px`) raises page padding→40px (`--space-10`)

## Deviations from Plan

None. Plan executed exactly as written. The two explicit decision points in the plan were addressed under "decisions" in frontmatter:

1. **Middot rendering** — chose HTML-inline (recommended in plan; selectable text + simpler CSS).
2. **Optional H2 underline accent** — skipped (allowed by UI-SPEC §Accent reserved-for-#3 as optional).

Both are documented decisions, not deviations from constraints.

## Threat surface

No new attack surface introduced. Stylesheet is local-only, contains no `@import`, no remote font URLs, no CDN references. Threats T-01-02-01 and T-01-02-02 (tampering via external CSS/fonts) are mitigated by absence — verified by negative grep gates.

## Files

### Created
- `style.css` (323 lines)

### Modified
- (none)

## Commits

| Hash | Type | Description |
|------|------|-------------|
| `0cea87a` | feat(01-02) | add shared style.css with UI-SPEC tokens, layout, news scroller |

## Self-Check: PASSED

Verified post-commit:
- `style.css` exists at `/Users/ivolinengong/Documents/My_Website/style.css` — confirmed via `test -f`
- Commit `0cea87a` exists in `git log --oneline` — confirmed
- All 14 grep verification gates listed above re-ran and PASS

## Next plan

Plan 01-03 (Home page `index.html`) consumes this stylesheet via `<link rel="stylesheet" href="style.css">` and uses the selectors listed in the "Selector contract for Plan 03" table.
