---
phase: 02-remaining-content-pages
plan: 03
subsystem: talks-page
tags:
  - static-site
  - content-extraction
  - talks-page
  - phase-2-wave-2
requires:
  - 02-01-SUMMARY.md
  - 01-UI-SPEC.md
  - "reference/wayback/Talks - Ivoline Ngong.html"
provides:
  - "talks.html at repo root rendering 3 invited talks verbatim with venue, year, and resource links"
  - "Live anchor target for the publications.html / index.html / code.html / writing.html nav item 'Talks'"
affects:
  - talks.html
tech-stack:
  added: []
  patterns:
    - "Site shell copied byte-identical from index.html, with two diffs: site-title loses aria-current; Talks nav item becomes <span aria-current=\"page\">"
    - ".talk-entry > .talk-body single-paragraph contract from Plan 02-01 used unchanged"
    - "Verbatim wayback extraction: strip web.archive.org/web/{ts}/ prefix, drop inline styles, drop target=_blank/rel=noopener, preserve &amp; and &nbsp;"
key-files:
  created:
    - path: talks.html
      purpose: "Talks page — 3 invited talks (Microsoft Research 2022, University of Buea 2022, UVM CS Research Day 2021)"
      lines: 40
  modified: []
decisions:
  - "Talk 1 retained the wayback's ` |&nbsp;` separator between Video and Feature links verbatim (literal pipe + non-breaking space) — matches the wayback's inline-prose formatting; not promoted to <span class=\"pub-sep\"> because the pipe lives inside running prose, not in a structured link row"
  - "Talk 2 retained the wayback's `&nbsp;` between the period and the Video link verbatim"
  - "Preserved `&amp;` HTML entity in Talk 1's YouTube URL (vs. converting to `&`) — wayback verbatim invariant"
  - "Page uses <h2>Talks</h2> as the first heading inside <main>, matching the type scale and the no-H1-in-Phase-2 convention inherited from Phase 1"
  - "Site-title link (.site-title href=\"index.html\") deliberately drops aria-current=\"page\" since this is not the Home page; Talks nav item is the sole aria-current target"
metrics:
  duration: "~1 minute"
  completed: 2026-05-19
  tasks_completed: 1
  files_created: 1
  files_modified: 0
  lines_added: 40
  lines_removed: 0
  commits: 1
---

# Phase 2 Plan 03: talks.html (Talks listing) Summary

Built `talks.html` at the repo root — a 40-line static page rendering the 3 invited talks recovered verbatim from `reference/wayback/Talks - Ivoline Ngong.html`, mounted on the Phase 1 site shell with the `Talks` nav item marked active and using the `.talk-entry` / `.talk-body` CSS classes added in Plan 02-01.

## What Was Built

A single new file at the repo root: **`/Users/ivolinengong/Documents/My_Website/talks.html`** (40 lines).

Document structure top-to-bottom:

1. `<!doctype html>` + standard `<head>` (charset, viewport, title, description, stylesheet link to `style.css`)
2. `<a class="skip-link" href="#main">Skip to main content</a>` — first body child
3. `<header class="site-header">` containing:
   - `<a class="site-title" href="index.html">Ivoline Ngong</a>` — **no** `aria-current` (this is not Home)
   - `<nav class="site-nav" aria-label="Site">` with 5 items in order: `Main` (a), `Papers` (a), `Talks` (**span** with `aria-current="page"`), `Code` (a), `Writing` (a)
4. `<main id="main">` containing:
   - `<h2>Talks</h2>` (first child)
   - 3 `<article class="talk-entry">` blocks in wayback source order

### The 3 talks (verbatim, post-strip)

| # | Title | Venue | Year | Resource Links |
|---|-------|-------|------|----------------|
| 1 | Compiler Optimizations using Reinforcement Learning | Microsoft Research | 2022 | [Video](https://www.youtube.com/watch?v=jnkxDdZF_gc&t=781s) (YouTube, `&amp;` preserved in href) · [Feature](https://www.microsoft.com/en-us/research/academic-program/rl-open-source-fest/alumni/) (Microsoft Research RLOS alumni page) |
| 2 | Fairness and Privacy in AI | University of Buea | 2022 | [Video](https://youtu.be/9zFhJ5vjBtw) (YouTube short link) |
| 3 | Continual Audit of Individual and Group Fairness in Deployed Classifiers via Prediction Sensitivity | CS Research Day, University of Vermont | 2021 | [Slides](https://drive.google.com/file/d/1TLQXATVglRZYbVRz73XNA2Kn0xLIK-4x/view?usp=sharing) (Google Drive) |

Source-text typos preserved verbatim: **none observed** in the talks block. (The 3 paragraphs in `reference/wayback/Talks - Ivoline Ngong.html` lines 684/688/692 are clean — no spelling issues, no malformed punctuation, no orphaned commas.)

## Why

Phase 2 Wave 2 ships the four destination pages (Papers / Talks / Code / Writing) that Wave 1's Plan 02-01 hooked the Home nav to. The Talks page is the structurally simplest of the four — 3 single-paragraph entries against the pre-declared `.talk-entry` contract — so it fits cleanly into a single executor task with no checkpoints. Mounting the same site-header markup on every Phase 2 page (only the page `<title>`, `<meta description>`, and the aria-current target differ) is what makes the four Wave 2 plans independently buildable in parallel.

## How

Single Task 1 (single commit `f989831`):

1. Copied the site-shell pattern from `index.html` lines 1-22, applying three diffs:
   - `<title>` changed to `Talks — Ivoline Ngong` (em-dash, matches UI-SPEC §Copywriting Contract)
   - `<meta name="description">` set to `Invited talks by Ivoline Ngong on privacy, security, fairness, and AI.` (period-terminated for parity with `publications.html`)
   - `.site-title` loses `aria-current="page"`; nav item `Talks` becomes `<span aria-current="page">Talks</span>` while `Main`/`Papers`/`Code`/`Writing` become regular `<a href>` anchors
2. Below the header, inside `<main id="main">`, emitted `<h2>Talks</h2>` followed by 3 `<article class="talk-entry">` blocks, each wrapping a single `<p class="talk-body">` containing title + venue + year + inline resource links — exactly matching the `.talk-entry` markup contract from Plan 02-01.
3. For each talk's prose, extracted the wayback `<p>` paragraph verbatim (from `reference/wayback/Talks - Ivoline Ngong.html` lines 684/688/692) and applied the Phase 2 stripping invariants:
   - **web.archive.org prefix:** removed from all 4 hrefs (3 talks × 1-2 links each = 4 anchors total)
   - **inline styles:** dropped `style="background-color: #ffffff; color: #009688;"` (Talk 1) and `style="color: #009688;"` (Talks 2 & 3) — global `a { color: var(--accent); }` rule in `style.css` handles link coloring
   - **target/rel attributes:** dropped all `target="_blank"` and `rel="noopener"` attributes (Phase 1 invariant — zero `target="_blank"` anywhere)
   - **HTML entities:** kept `&amp;` (Talk 1's YouTube URL ampersand) and `&nbsp;` (Talk 1 separator, Talk 2 prefix) verbatim
4. Closed with `</main></body></html>` and a single trailing newline. No `<footer>`, no `<script>`, no inline `<style>` block.

## Verification

All 28 automated gates from the plan's `<verify><automated>` block pass on first run:

```
PASS: talks.html written with 3 verbatim entries
```

Itemized gate results:

| # | Gate | Status |
|---|------|--------|
| 1 | File exists at repo root | OK |
| 2 | `<!doctype html>` present (case-insensitive) | OK |
| 3 | `<title>Talks — Ivoline Ngong</title>` exact match | OK |
| 4 | `href="style.css"` stylesheet link | OK |
| 5 | `class="skip-link"` present | OK |
| 6 | `<a class="site-title" href="index.html">Ivoline Ngong</a>` (no aria-current) | OK |
| 7 | `<a href="index.html">Main</a>` nav link | OK |
| 8 | `<a href="publications.html">Papers</a>` nav link | OK |
| 9 | `<span aria-current="page">Talks</span>` — sole aria-current | OK |
| 10 | `<a href="code.html">Code</a>` nav link | OK |
| 11 | `<a href="writing.html">Writing</a>` nav link | OK |
| 12 | `<h2>Talks</h2>` page heading | OK |
| 13 | Exactly 3 `<article class="talk-entry">` blocks | OK (3) |
| 14 | Exactly 3 `<p class="talk-body">` paragraphs | OK (3) |
| 15 | Talk 1 title prose present | OK |
| 16 | Talk 2 title prose present | OK |
| 17 | Talk 3 title prose present | OK |
| 18 | YouTube URL (Talk 1) preserved | OK |
| 19 | youtu.be URL (Talk 2) preserved | OK |
| 20 | Google Drive URL (Talk 3) preserved | OK |
| 21 | Microsoft RLOS URL (Talk 1) preserved | OK |
| 22 | Zero `web.archive.org` substrings | OK |
| 23 | Zero `target="_blank"` attributes | OK |
| 24 | Zero `rel="noopener"` attributes | OK |
| 25 | Zero `<script` tags | OK |
| 26 | Zero `style="background-color` inline styles | OK |
| 27 | Zero `style="color` inline styles | OK |
| 28 | No `<a>` element carries `aria-current` | OK |
| — | Line count ≥ 30 | OK (40 lines) |

End-of-plan verification block also clean: `VERIFICATION COMPLETE` with no `FAIL:` markers, including all 4 cross-page nav-consistency checks (`Main:index`, `Papers:publications`, `Code:code`, `Writing:writing`).

## Acceptance Criteria

All 12 criteria from the plan's Task 1 acceptance block satisfied:

- [x] `talks.html` exists at repo root
- [x] `<title>` is exactly `Talks — Ivoline Ngong`
- [x] Skip link is the first body child
- [x] `<header class="site-header">` contains site-title link to `index.html` WITHOUT `aria-current`
- [x] Nav has 5 children in order Main / Papers / Talks / Code / Writing
- [x] `Talks` is the ONLY nav item carrying `aria-current="page"`, and it is a `<span>`
- [x] Exactly 3 `<article class="talk-entry">` elements, each containing exactly one `<p class="talk-body">`
- [x] Talk 1 contains both the YouTube Video link AND the Microsoft Feature link
- [x] Talk 2 contains the youtu.be Video link
- [x] Talk 3 contains the Google Drive Slides link
- [x] Zero `web.archive.org`, zero `target="_blank"`, zero `rel="noopener"`, zero `<script>`, zero `style="…"` inline styles
- [x] No anchor carries `aria-current`
- [x] File ends with `</main></body></html>` and a single trailing newline

## Threat-Model Dispositions Honored

| Threat ID | Disposition | Honored | Evidence |
|-----------|-------------|---------|----------|
| T-02-03-01 | accept (owner-curated external links) | n/a | Links point to youtube.com, youtu.be, drive.google.com, microsoft.com — exactly the owner-published set |
| T-02-03-02 | mitigate (Wayback prefix stripping) | OK | `grep -c web.archive.org talks.html` returns 0 |
| T-02-03-03 | mitigate (no tabnabbing) | OK | `grep -c target="_blank" talks.html` returns 0 |
| T-02-03-04 | accept (publicly delivered talks) | n/a | All 3 talks were publicly given; no PII beyond owner-published content |
| T-02-03-05 | mitigate (verbatim no fabrication) | OK | All 3 entries cross-checked against wayback lines 684/688/692; titles/venues/years copied byte-for-byte |
| T-02-03-06 | accept (page weight) | n/a | 40-line static HTML, zero images, zero scripts |
| T-02-03-SC | n/a (no packages) | n/a | This plan installs no packages |

## Commits

| Hash | Type | Files | Description |
|------|------|-------|-------------|
| `f989831` | feat(02-03) | talks.html | Add talks.html with 3 verbatim wayback entries |

## Deviations from Plan

None — plan executed exactly as written. Task 1's automated verification gate passed on the first run (28/28 sub-gates). No deviation rules (1, 2, 3, or 4) triggered. No auth gates encountered. No checkpoints in this plan.

## Threat Flags

None. This plan adds a single same-origin static HTML file whose only outbound surface is 4 external anchors (3 talks × 1-2 links) to owner-curated destinations (YouTube, Google Drive, Microsoft Research). No new auth paths, no file-access patterns, no schema or trust-boundary changes. Threat mitigation T-02-03-03 (tabnabbing) verified: `grep -c target="_blank" talks.html` returns 0.

## Known Stubs

None. All 3 talk entries are complete (title + venue + year + at least one resource link), all 4 external URLs are canonical (not Wayback-prefixed), and the page renders fully without any placeholder text. The nav anchors to `code.html` and `writing.html` will 404 until their respective Wave-2 sibling plans (02-04 writing, 02-05 code) ship — this is *not* a stub but the explicit Wave-2 parallel execution contract established in 02-01-SUMMARY.

## Self-Check: PASSED

Files claimed in this SUMMARY verified to exist on disk and at the expected commits:

- `/Users/ivolinengong/Documents/My_Website/talks.html` — FOUND (40 lines, contains all 28 verified content markers above)
- Commit `f989831` — FOUND on `main`, subject `feat(02-03): add talks.html with 3 verbatim wayback entries`, 1 file changed, 40 insertions
- All 28 automated verification gates — PASS
- All 7 cross-page nav-consistency end-of-plan gates — PASS
- Threat-model line-by-line dispositions — all 7 honored
