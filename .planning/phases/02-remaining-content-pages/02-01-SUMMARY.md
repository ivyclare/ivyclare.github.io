---
phase: 02-remaining-content-pages
plan: 01
subsystem: site-shell
tags:
  - static-site
  - nav
  - css
  - phase-2-foundation
requires:
  - 01-03-SUMMARY.md
  - 01-UI-SPEC.md
provides:
  - "Active <a href> nav anchors on index.html pointing at publications.html / talks.html / code.html / writing.html"
  - "Content-entry CSS classes (.pub-entry, .talk-entry, .writing-year-group, .writing-entry, .code-intro) usable by Wave-2 plans"
affects:
  - index.html
  - style.css
tech-stack:
  added: []
  patterns:
    - "Hairline border-bottom + :last-child reset on entry containers (matches .news-entry)"
    - "Mobile font-size overrides consolidated into a single @media (max-width: 639px) block at the end of the content-entry section"
key-files:
  created: []
  modified:
    - path: index.html
      change: "Replaced 4 <span data-todo='phase-2'> placeholders with real <a href> anchors (lines 17-20)"
    - path: style.css
      change: "Appended 118-line content-entry section after Reduced-motion block; no edits to existing rules"
decisions:
  - "Kept Main item as <span aria-current='page'> on Home (no self-link), per UI-SPEC active-page rule"
  - "Retained dormant .site-nav [data-todo='phase-2'] CSS rule per plan instructions (one or more future spans may reuse it)"
  - "Consolidated all mobile (16px) overrides into a single @media block at section bottom — keeps cascade flat for future audits"
  - "Used existing tokens exclusively (--accent, --text, --text-strong, --text-muted, --border, --space-*); no new hex values or font weights introduced"
metrics:
  duration: "~2.3 minutes"
  completed: 2026-05-19
  tasks_completed: 2
  files_modified: 2
  lines_added: 122
  lines_removed: 4
  commits: 2
---

# Phase 2 Plan 01: Foundation — Activate Nav + Content-Entry CSS Summary

Activated the four Phase-1 nav placeholders on the Home page into real same-origin `<a href>` anchors targeting `publications.html`, `talks.html`, `code.html`, and `writing.html`, and appended a 5-family content-entry stylesheet block (.pub-entry, .talk-entry, .writing-year-group, .writing-entry, .code-intro) to `style.css` so the four Wave-2 page-build plans can render their pages without inventing new CSS conventions.

## What Was Built

**1. Activated Phase-2 nav anchors (`index.html`, lines 17-20).** Replaced four `<span data-todo="phase-2">` placeholders with real `<a href>` anchors per the UI-SPEC label→file mapping. The existing `.site-nav a` rule paints these in `--accent` (teal) automatically — no CSS change was needed for the nav itself. The `<span aria-current="page">Main</span>` on line 16 was deliberately left as a non-anchor span (Home does not self-link).

Exact line-by-line diff (line numbers refer to post-edit file):

| Line | Before | After |
|------|--------|-------|
| 17 | `      <span data-todo="phase-2">Papers</span>` | `      <a href="publications.html">Papers</a>` |
| 18 | `      <span data-todo="phase-2">Talks</span>` | `      <a href="talks.html">Talks</a>` |
| 19 | `      <span data-todo="phase-2">Code</span>` | `      <a href="code.html">Code</a>` |
| 20 | `      <span data-todo="phase-2">Writing</span>` | `      <a href="writing.html">Writing</a>` |

Line count of `index.html` unchanged (143 lines). Whitespace and indentation preserved exactly. No `target="_blank"` introduced.

**2. Appended content-entry CSS classes (`style.css`, lines 423-539).** Added a single 118-line section after the existing Reduced-motion block at end-of-file, declaring the five class families that Wave-2 plans will render against:

| Selector | Purpose |
|----------|---------|
| `.pub-entry` | Publication container; margin-block-end + 1px hairline + `:last-child` reset |
| `.pub-entry .pub-title` | Paper title — 17px (16px mobile), `--text-strong`, line-height 1.4, font-weight 400 |
| `.pub-entry .pub-venue` | Authors + venue line — 15px, `--text-muted`, line-height 1.5 |
| `.pub-entry .pub-links` | Row of resource links — 15px |
| `.pub-entry .pub-links .pub-sep` | Pipe separator between links — `--text-muted`, margin 0 var(--space-2) |
| `.pub-entry .pub-tldr` | Optional italic TLDR — 15px, `--text`, line-height 1.55, font-style italic |
| `.talk-entry` | Talk container; same margin/hairline pattern as `.pub-entry` |
| `.talk-entry .talk-body` | Talk title+venue+year+links body — 17px (16px mobile) |
| `.writing-year-group` | Wraps each year's entries; margin-block-end var(--space-10) + `:last-child` reset |
| `.writing-year-group h3` | Year heading override — sets size + margin only; inherits small-caps and color from global `h3` rule |
| `.writing-entry` | Writing entry container; margin-block-end var(--space-6) |
| `.writing-entry .writing-body` | Writing entry body — 17px (16px mobile) |
| `.code-intro` | Code page intro paragraph — 17px (16px mobile), `max-width: 60ch` for readability |

All mobile font-size overrides (4 selectors that step 17px→16px under `@media (max-width: 639px)`) consolidated into a single media block at section bottom. Zero hover transitions, animations, gradients, shadows, or new color/weight values.

## Why

Phase 1 shipped Home with four `<span data-todo="phase-2">` placeholders for nav items whose destination pages did not yet exist — that was the right call at the time, since hooking up broken anchors would have produced 404s. Wave 1 of Phase 2 flips them to real anchors so that the four content-page plans (02-02 publications, 02-03 talks, 02-04 writing, 02-05 code) can ship their pages independently in Wave 2 without requiring a separate "patch the nav" plan after each. The CSS-class additions follow the same logic: pre-declaring the entry-family rules gives the four downstream plans a stable contract to render against, avoiding ad-hoc class invention that would later need consolidation.

## How

- **Task 1 — Nav swap (commit `52a6fb5`).** Single multi-line `Edit` against lines 16-20 of `index.html`, replacing the four placeholder spans with their corresponding `<a href>` anchors per the UI-SPEC `Nav label → file mapping` table. The Main `<span aria-current="page">` and surrounding `<nav class="site-nav">` markup were left untouched. Verification: `grep -c 'data-todo="phase-2"' index.html` returned 0, all four `href="*.html">LABEL</a>` patterns matched, Main span still present, and `target="_blank"` absent from the file.
- **Task 2 — CSS append (commit `8fbbfa2`).** Single append after the Reduced-motion block: 117 new lines + 1 trailing newline. Used `:last-child` resets on `.pub-entry`, `.talk-entry`, `.writing-year-group`, and `.writing-entry` to suppress the trailing hairline/margin (matches the existing `.news-entry:last-child` pattern). Used only declared CSS custom properties; verified by hex-color audit (only the 8 declared palette colors appear in the file) and font-weight audit (all three weights in the file are `400`).

## Trade-offs

- **Kept the dormant `.site-nav [data-todo="phase-2"]` rule (style.css line 160).** Per the plan, this is intentionally retained for forward compatibility — a future plan may re-introduce a placeholder span and benefit from the existing muted-gray styling. Two lines of dead CSS is a fair price for that optionality.
- **Mobile overrides consolidated rather than co-located.** Each entry-family declares its desktop font-size next to the other properties, then a single `@media (max-width: 639px)` block at the section bottom overrides the mobile sizes. Trade-off: rule-locality for cascade-flatness. Cascade-flatness wins here because the four overrides ride the same breakpoint and one media block is easier to audit than four scattered ones.
- **No hover transitions on entry containers.** The Phase 1 contract is Carlini-minimal (no transitions beyond `text-decoration-thickness` on links). Honored that here — links inside entries inherit the global teal-underline hover from `a:hover`, but the containers themselves have no hover state.

## Phase 1 Invariant Audit (No Regressions)

Verified each Phase 1 invariant is preserved:

| Invariant | Status | Evidence |
|-----------|--------|----------|
| No `target="_blank"` anywhere in `index.html` | OK | `grep target="_blank" index.html` returns nothing |
| `.site-header`, `.bio-layout`, `.profile-card`, `.news-list`, `.news-entry`, `.news-date` rules unchanged | OK | Diff is pure 118-line addition; existing rules untouched (verified by `git diff --cached --stat`: 118 insertions, 0 deletions) |
| `.news-date` retains teal color (`--accent`) | OK | Line 376 of style.css unchanged |
| Hairline divider between news entries (`rgba(229,231,235,0.55)`) preserved | OK | Line 357 unchanged |
| Wayback content fidelity in `index.html` (bio paragraphs, news entries) | OK | Only lines 17-20 modified |
| Single font-weight invariant (400 everywhere) | OK | Only three `font-weight:` declarations in the file, all `400` |
| Type scale (15/17/22/28 desktop, 14/16/24 mobile) honored | OK | New rules use only 15px (small), 17px (body) desktop; 16px mobile |
| 8-color palette unchanged | OK | Hex audit shows only `#FFFFFF`, `#1F2937`, `#111827`, `#6B7280`, `#0F766E`, `#E5E7EB`, `#FAFAFA`, `#D1D5DB` — exactly the declared set |
| File ends with single trailing newline | OK | `tail -c 5 style.css` shows `}\n}\n` |
| `index.html` line count unchanged (143) | OK | Replacements were strict 1-for-1 |

## Verification

All gates from the plan's `<verification>` block return OK:

```
=== Nav activation gates ===
  OK: placeholders=0
  OK: Papers anchor
  OK: Talks anchor
  OK: Code anchor
  OK: Writing anchor
  OK: Main marker
  OK: no target=_blank

=== CSS selector gates (12 selectors) ===
  OK: .pub-entry
  OK: .pub-entry .pub-title
  OK: .pub-entry .pub-venue
  OK: .pub-entry .pub-links
  OK: .pub-entry .pub-tldr
  OK: .talk-entry
  OK: .talk-entry .talk-body
  OK: .writing-year-group
  OK: .writing-entry
  OK: .writing-entry .writing-body
  OK: .code-intro

=== Invariant gates ===
  OK: only font-weight 400
  OK: palette unchanged
```

Total: 21/21 automated gates PASS.

**Visual eyeball check (deferred — Wave-2 plans will exercise it):** Opening `file:///Users/ivolinengong/Documents/My_Website/index.html` should show 5 teal small-caps nav items in order Main / Papers / Talks / Code / Writing. Clicking Papers/Talks/Code/Writing will currently 404 until plans 02-02 through 02-05 ship their destination HTML files — this is expected and is the explicit Wave-1/Wave-2 sequencing of Phase 2.

## Commits

| Hash | Type | Files | Description |
|------|------|-------|-------------|
| `52a6fb5` | feat(02-01) | index.html | Activate Phase-2 nav anchors in index.html |
| `8fbbfa2` | feat(02-01) | style.css | Add content-entry CSS classes for Wave-2 plans |

## Deviations from Plan

None — plan executed exactly as written. Each task hit its automated verification gate on the first run. No deviation rules (1, 2, 3, or 4) triggered. No auth gates encountered. No checkpoints in this plan.

## Threat Flags

None. This plan introduces only same-origin internal `<a href>` anchors and additive CSS rules. No new network endpoints, no auth paths, no file-access patterns, no schema changes at any trust boundary. Threat mitigation T-02-01-03 (tabnabbing) is satisfied: zero `target="_blank"` attributes in `index.html`, verified by automated grep.

## Known Stubs

None. The four anchors point to files that do not yet exist (publications.html, talks.html, code.html, writing.html) — this is *not* a stub but the explicit Wave-1/Wave-2 contract of Phase 2 (the destinations land in plans 02-02..02-05). The dormant `.site-nav [data-todo="phase-2"]` CSS rule retained at line 160 of `style.css` is *not* a stub either: it is a declared dead-code retention per the plan's instructions, with zero current renderable surface.

## Self-Check: PASSED

Files claimed in this SUMMARY verified to exist on disk and at the expected commits:

- `/Users/ivolinengong/Documents/My_Website/index.html` — exists, 143 lines, nav block lines 13-22 contains 4 `<a href>` anchors + 1 `<span aria-current="page">Main</span>`.
- `/Users/ivolinengong/Documents/My_Website/style.css` — exists, 539 lines, content-entry section spans lines 423-539.
- Commit `52a6fb5` — present in `git log --oneline` on `main`, subject `feat(02-01): activate Phase-2 nav anchors in index.html`.
- Commit `8fbbfa2` — present in `git log --oneline` on `main`, subject `feat(02-01): add content-entry CSS classes for Wave-2 plans`.
- All 12 CSS selectors from Task 2 acceptance criteria — present (verified by grep).
- All 7 nav-activation gates — pass.
- All 2 invariant gates (font-weight 400 only, palette unchanged) — pass.
