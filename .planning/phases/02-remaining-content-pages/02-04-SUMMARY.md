---
phase: 02-remaining-content-pages
plan: 04
subsystem: writing-page
tags:
  - static-site
  - content-extraction
  - writing-page
requires:
  - 02-01-SUMMARY.md
  - 01-03-SUMMARY.md
  - 01-UI-SPEC.md
provides:
  - "writing.html — year-grouped Writing listing with 6 entries"
affects:
  - writing.html
tech-stack:
  added: []
  patterns:
    - "Year-grouped content listing using .writing-year-group + .writing-entry classes from Plan 02-01"
    - "CONT-09 broken-anchor-drop pattern: drop <a> wrapping for WordPress placeholder URLs (ivolinengong.com/748-2, ivolinengong.com/hello-world); retain entry text inline"
    - "Heading-level shift: wayback <h2> year headings demoted to <h3> so the page document outline stays h2 → h3 (page-level <h2> reserved for 'Writing' per UI-SPEC §Typography)"
key-files:
  created:
    - path: writing.html
      purpose: "Writing page — blog post listing grouped by year (2023, 2022, 2021, 2019)"
  modified: []
decisions:
  - "Dropped 3 broken WordPress-placeholder anchors per Phase 1 D-04 (CONT-09): 2021 Comic – PATE Analysis (ivolinengong.com/748-2), 2019 Deep CARs (ivolinengong.com/hello-world), 2019 Population vs Sample (ivolinengong.com/hello-world). Entry text retained inline."
  - "Used <a href=\"index.html\">Main</a> in the nav (Main is a real anchor on non-home pages) and <span aria-current=\"page\">Writing</span> for the active page; removed aria-current from .site-title on this page (it is reserved for Home per Phase 1 convention)."
  - "Preserved source-text quirks verbatim: em-dash glued to 'CARs' (no space) in 'Deep CARs— Transfer Learning With Pytorch'; space-before-comma in 'Population vs Sample , Statistic vs Parameter'; curly quotes in 'PySyft \u201cDomain\u201d servers'."
  - "Preserved &nbsp; HTML entity after the 2022 and 2021-first entries' closing </a> for source fidelity (wayback source used &nbsp; for spacing)."
metrics:
  duration: "~3 minutes"
  completed: 2026-05-19
  tasks_completed: 1
  files_modified: 1
  lines_added: 62
  lines_removed: 0
  commits: 1
---

# Phase 2 Plan 04: writing.html (Writing listing) Summary

Built `writing.html` at repo root rendering 6 writing entries extracted verbatim from `reference/wayback/Writing - Ivoline Ngong.html` and grouped into 4 year sections (2023, 2022, 2021, 2019) on the shared site shell, with the `Writing` nav item marked as the current page and 3 broken WordPress-placeholder anchors dropped (text retained) per CONT-09.

## What Was Built

**`writing.html`** — 62-line static HTML file using the Phase 1 site shell (skip-link, `.site-header`, `.site-nav`, `<main id="main">`) plus the `.writing-year-group` / `.writing-entry` / `.writing-body` classes from Plan 02-01. No new CSS introduced. Page outline:

- `<title>Writing — Ivoline Ngong</title>`
- `<meta name="description" content="Writing by Ivoline Ngong — blog posts on differential privacy, fairness audits, and AI.">`
- `<a class="site-title" href="index.html">Ivoline Ngong</a>` (no `aria-current`)
- Nav: `Main` (anchor) · `Papers` (anchor) · `Talks` (anchor) · `Code` (anchor) · `Writing` (`<span aria-current="page">`)
- `<h2>Writing</h2>` first child of `<main>`
- 4 `<section class="writing-year-group">` blocks in source order

### Entries extracted (6 total)

| Year | Title | URL in final HTML | Description retained |
|------|-------|--------------------|----------------------|
| 2023 | How To Audit An AI Model Owned by Someone else (Part 1) | https://blog.openmined.org/ai-audit-part-1/ | yes |
| 2022 | Prediction Sensitivity: Continual Audit of Counterfactual Fairness in Deployed Classifiers | https://montrealethics.ai/prediction-sensitivity-continual-audit-of-counterfactual-fairness-in-deployed-classifiers/ | yes |
| 2021 | Maintaining Privacy in Medical Data with Differential Privacy | https://blog.openmined.org/maintaining-privacy-in-medical-data-with-differential-privacy/ | yes |
| 2021 | Comic – PATE Analysis | **DROPPED (placeholder)** — original: https://ivolinengong.com/748-2/ | yes (text only) |
| 2019 | Deep CARs— Transfer Learning With Pytorch | **DROPPED (placeholder)** — original: https://ivolinengong.com/hello-world/ | yes (text only) |
| 2019 | Population vs Sample , Statistic vs Parameter | **DROPPED (placeholder)** — original: https://ivolinengong.com/hello-world/ | yes (text only) |

### Broken anchors dropped (3 — flagged for owner follow-up per CONT-09)

The wayback source pointed three entries at WordPress default/placeholder URLs that 404 on the dead `ivolinengong.com` host. Per Phase 1 D-04 (`news-broken-links` / `bio-broken-links` from `01-03-SUMMARY`) these are rendered as text-only prose with no anchor; entry titles and descriptions are retained verbatim.

| # | Year | Entry | Original (broken) URL | Reason |
|---|------|-------|------------------------|--------|
| 1 | 2021 | Comic – PATE Analysis | `https://ivolinengong.com/748-2/` | WordPress slug self-reference on dead host (404) |
| 2 | 2019 | Deep CARs— Transfer Learning With Pytorch | `https://ivolinengong.com/hello-world/` | WordPress default placeholder URL (404) |
| 3 | 2019 | Population vs Sample , Statistic vs Parameter | `https://ivolinengong.com/hello-world/` | Same `hello-world` placeholder URL (duplicate placeholder) |

**Owner-action recommendation:** These three entries probably exist at real canonical URLs elsewhere — likely Medium archives, OpenMined blog archives, or the owner's personal notes. If the owner can supply the real URLs in a one-line follow-up, a 5-minute patch plan can re-attach the anchors. Until then, the entries surface as readable prose (preferable to misleading clickable 404s).

## Why

Phase 2 deliverable PAGE-04 requires `writing.html` to ship as part of the Wave-2 content pages. The wayback source presents entries grouped under `<h2>` year headings — we re-express that structure as `<h3>` year subheadings inside `<section class="writing-year-group">` wrappers, with the page-level `<h2>` reserved for the section title `Writing` (matches UI-SPEC §Typography: h2 = section heading, h3 = sub-section). The 6 entries land verbatim per CONT-07.

## How

- **Task 1 (commit `d42b0a8`).** Wrote `writing.html` in a single `Write` call: site shell + 4 year-group sections + 6 entries. Stripped Wayback prefixes (`https://web.archive.org/web/{timestamp}/`), inline `style="color: #009688;"` and `style="background-color: #ffffff; color: #009688;"` attributes (global `a { color: var(--accent); }` rule from Phase 1 handles teal coloring), `target="_blank"`, and `rel="noopener"` attributes. Dropped `<a>` wrapping for 3 broken-URL entries.

## Trade-offs

- **Preserved `&nbsp;` literal entities** after the 2022 and first-2021 entries' closing `</a>` rather than collapsing to a normal space. Trade-off: source fidelity vs. minor markup noise. Source-fidelity wins — these match the wayback rendering exactly and visually collapse identically to ` ` in the browser.
- **Heading-level shift (wayback `<h2>` year heads → `<h3>` in our markup).** Trade-off: divergence from the wayback DOM structure vs. correct document outline. Correct-outline wins — `<h2>Writing</h2>` is the section heading per UI-SPEC, so years must demote to `<h3>` to preserve the h2 → h3 cascade. The existing `style.css` `h1, h2, h3` rule handles styling.
- **Removed `aria-current="page"` from `.site-title` on this page.** On Home it is set (`index.html` line 14). On non-home pages we treat the title as a return-to-home link, and the current-page marker lives only on the matching nav item. This matches the Phase 1 convention where the active marker is a single element per page.

## Phase 1 / 02-01 Invariant Audit (No Regressions)

| Invariant | Status | Evidence |
|-----------|--------|----------|
| No `<script>` tag | OK | `grep -c '<script' writing.html` returns 0 |
| No inline `style=` | OK | `grep -c 'style="'` returns 0 |
| No `target="_blank"` / `rel="noopener"` | OK | Both `grep -c` return 0 |
| No `web.archive.org` prefix | OK | `grep -c web.archive.org` returns 0 |
| No `<a aria-current=>` | OK | Active marker is `<span aria-current="page">Writing</span>` only |
| Single trailing newline | OK | `tail -c 2 writing.html` shows `>\n` |
| `.writing-year-group` + `.writing-entry` classes from 02-01 used as declared | OK | 4 year-groups, 6 writing-entries, 6 writing-body paragraphs |

## Verification

All 35 automated gates from the plan's `<verify><automated>` block pass:

```
PASS: writing.html written with 6 entries across 4 year-groups, broken anchors dropped
```

End-of-plan verification block also clean:

```
VERIFICATION COMPLETE
```

## Commits

| Hash | Type | Files | Description |
|------|------|-------|-------------|
| `d42b0a8` | feat(02-04) | writing.html | Build writing.html with year-grouped entries |

## Deviations from Plan

None — plan executed exactly as written. All 35 automated gates passed on the first run. No deviation rules (1, 2, 3, or 4) triggered. No auth gates encountered. No checkpoints in this plan.

## Threat Flags

None new. The 3 active outbound links cross to `blog.openmined.org` (×2) and `montrealethics.ai` (×1) — all owner-curated canonical destinations per T-02-04-01 (accept). Tabnabbing mitigation T-02-04-03 satisfied (zero `target="_blank"`). Wayback-prefix injection T-02-04-02 satisfied (zero `web.archive.org`). Broken-placeholder mitigation T-02-04-04 satisfied: 3 dead-host URLs dropped with text retained, flagged above per T-02-04-05 / CONT-09. No new endpoints, auth paths, or file-access patterns.

## Known Stubs

None. The 3 placeholder-dropped entries are documented broken-URL flags (with owner-action recommendation for follow-up URL recovery), not stubs — the entry text is real and renders. This matches the Phase 1 `news-broken-links` decision (D-04 in `01-03-SUMMARY`).

## Self-Check: PASSED

- `/Users/ivolinengong/Documents/My_Website/writing.html` — FOUND (62 lines, contains `<h2>Writing</h2>`, 4 `<section class="writing-year-group">`, 6 `<article class="writing-entry">`)
- Commit `d42b0a8` — FOUND in `git log --oneline` on `main`, subject `feat(02-04): build writing.html with year-grouped entries`
- 3 active canonical URLs present (`blog.openmined.org/ai-audit-part-1`, `montrealethics.ai/prediction-sensitivity-continual-audit-of-counterfactual-fairness-in-deployed-classifiers`, `blog.openmined.org/maintaining-privacy-in-medical-data-with-differential-privacy`) — verified by grep
- Zero broken-URL hrefs (`ivolinengong.com/(hello-world|748-2)`) — verified by grep
- All 35 automated verification gates — PASS
