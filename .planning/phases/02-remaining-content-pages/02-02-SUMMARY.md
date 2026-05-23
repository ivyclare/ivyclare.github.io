---
phase: 02-remaining-content-pages
plan: 02
subsystem: publications-page
tags:
  - static-site
  - content-extraction
  - publications-page
  - phase-2-content
requires:
  - 02-01-SUMMARY.md
  - 01-UI-SPEC.md
provides:
  - "publications.html at repo root — 7 wayback publications rendered with the .pub-entry CSS class family from Plan 02-01"
  - "Working cross-page nav from Publications back to Home/Talks/Code/Writing"
affects:
  - publications.html
tech-stack:
  added: []
  patterns:
    - "Single <article class='pub-entry'> per publication: pub-title, pub-venue, optional pub-links, optional pub-tldr"
    - "Reverse-chronological source order (NAACL 2025 → ACL Arxiv 2020) preserved from wayback"
    - "Multiple resource links separated by inline <span class='pub-sep'>|</span> (no whitespace around the span — matches the .profile-links separator pattern from index.html)"
key-files:
  created:
    - path: publications.html
      purpose: "Standalone Publications page with 7 entries verbatim from wayback source"
  modified: []
decisions:
  - "Dropped two ivolinengong.com/hello-world placeholder Paper links (entries 6 Heart Disease + 7 Towards auditability) per the constraint to drop broken WordPress placeholder anchors without fabricating canonical URLs"
  - "Preserved entry 7's Posters Google-Docs link (real, points to a real presentation) — gives entry 7 one valid resource link"
  - "Kept entry 6 (Heart Disease) with no .pub-links row — the title + venue + TLDR still convey content; flagged as needing-real-Paper-URL"
  - "Preserved source-text typos verbatim per Phase 1 D-05: SOUPS venue missing space before parenthesis, Arxiv venue with comma-year format, Springer venue with trailing period, entry-7 venue missing <br> between authors and venue"
  - "Entry 1 (NAACL 2025) venue line in the source has plain text 'NAACL 2025' (no <i> wrapper); the other 6 entries' venues are wrapped in <i>...</i>. Preserved verbatim — the .pub-venue CSS class styles the line uniformly regardless of the inner italic markup"
metrics:
  duration: "~6 minutes"
  completed: 2026-05-19
  tasks_completed: 2
  files_created: 1
  files_modified: 0
  lines_added: 88
  lines_removed: 0
  commits: 2
---

# Phase 2 Plan 02: publications.html (Publications Listing) Summary

Created `publications.html` at the repo root rendering all 7 wayback-archived publications verbatim — titles, authors, venues, TLDRs, and canonical-URL resource links — on the shared Phase-1 site shell, using the `.pub-entry` CSS class family pre-declared by Plan 02-01.

## What Was Built

**1. `publications.html` (Task 1 — commit `3a6c902`).** Site shell scaffolded as 28 lines: doctype, head with `<title>Publications — Ivoline Ngong</title>` (em-dash) + matching meta description + `<link rel="stylesheet" href="style.css">`, body with the visually-hidden skip-link, `<header class="site-header">` containing site-title (Home link, NO `aria-current`) + `<nav class="site-nav">` (5 children: real `<a>` for Main/Talks/Code/Writing, `<span aria-current="page">Papers</span>` marking the active page), and `<main id="main">` containing `<h2>Publications</h2>` as the page lede.

**2. Seven `<article class="pub-entry">` blocks (Task 2 — commit `e3509e6`).** Each entry extracted verbatim from `reference/wayback/Publications - Ivoline Ngong.html` in wayback source order (which is reverse-chronological). Per-entry contents:

| # | Title | Authors | Venue | Paper URL | Other Links | TLDR |
|---|-------|---------|-------|-----------|-------------|------|
| 1 | Differentially Private Learning Needs Better Model Initialization and Self-Distillation | Ngong, I. C., Near, J. P., & Mireshghallah, N. | NAACL 2025 (plain text, no italic wrap in source) | `https://aclanthology.org/2025.naacl-long.455/` | — | yes |
| 2 | Evaluating the Usability of Differential Privacy Tools with Data Practitioners | Ngong, I. C., Stenger, B., Near, J. P., & Feng, Y. | *Symposium of Usable Privacy and Security(SOUPS 2024)&nbsp; co-located with USENIX* | `https://www.usenix.org/system/files/soups2024-ngong.pdf` | — | yes |
| 3 | OLYMPIA: A Simulation Framework for Evaluating the Concrete Scalability of Secure Aggregation Protocols | Ngong, I. C., Gibson, N., & Near, J. P. | *2024 IEEE Conference on Secure and Trustworthy Machine Learning (SaTML)* | `https://ieeexplore.ieee.org/abstract/document/10516647` | Code: `https://github.com/uvm-plaid/olympia` | yes |
| 4 | Different Deep Learning Based Classification Models for COVID-19 CT-Scans and Lesion Segmentation Through the cGAN-UNet Hybrid Method | Ngong, I. C., & Baykan, N. A. | *Traitement du Signal, 2023* | `https://www.proquest.com/openview/dd9a6b095d7a8bc3f3d331f47277dfab/1?pq-origsite=gscholar&cbl=2069443` | — | yes |
| 5 | Prediction Sensitivity: Continual Audit of Counterfactual Fairness in Deployed Classifiers | K Maughan, IC Ngong and JP Near | *Arxiv, 2022* | `https://arxiv.org/pdf/2202.04504.pdf` | Blog: `https://montrealethics.ai/prediction-sensitivity-continual-audit-of-counterfactual-fairness-in-deployed-classifiers/` | yes |
| 6 | Feature Extraction Methods for Predicting the Prevalence of Heart Disease | IC Ngong, NA Baykan | *Springer, Cham, 2021.* | **dropped — wayback source has `hello-world` placeholder** | — | yes |
| 7 | Towards auditability for fairness in deep learning | IC Ngong, K Maughan, JP Near | *ACL Arxiv 2020* | **dropped — wayback source has `hello-world` placeholder** | Posters: `https://docs.google.com/presentation/d/1yuvh1llMZkwdxAqBOLZJgYpQY-SZWZyX/edit?usp=sharing&ouid=100698771252276071384&rtpof=true&sd=true` | yes |

Resource-link counts: 6 of 7 entries have a `<p class="pub-links">` row (only entry 6 has no working link). All 7 entries have a `<p class="pub-tldr">` row with the source's surrounding curly-quote pair preserved verbatim.

## Why

Phase 2 deliverable. The wayback Publications page is ~1700 lines of nested WordPress + Elementor div-soup wrapping the actual content; this plan extracted the content layer (titles, authors, venues, real URLs, TLDR text) and rendered it in plain semantic HTML using the `.pub-entry` rules pre-declared in Plan 02-01. The original site's interactive TLDR/Citation toggle (JS-driven `.view` blocks) was dropped per the Phase 1 "no scripts" invariant; TLDRs are now inlined as static italic paragraphs (`.pub-tldr` is `font-style: italic` per the CSS contract). Citation BibTeX blocks were intentionally omitted — the canonical Paper URLs carry the citation upstream.

## How

- **Task 1 — Shell.** Single `Write` of a 28-line scaffold modelled on `index.html` lines 1-24. The only deltas from index.html: `<title>` swapped to `Publications — Ivoline Ngong`, meta description swapped, `aria-current="page"` removed from `.site-title` (Home self-link no longer applies), Main demoted from `<span aria-current="page">` to `<a href="index.html">Main</a>`, and the active marker moved to `<span aria-current="page">Papers</span>` in the nav.
- **Task 2 — Entries.** Single `Edit` injecting 60 lines between `<h2>Publications</h2>` and `</main>`. Each entry hand-extracted from the wayback source by line-anchored reading (the planner provided exact line numbers; the executor cross-checked title strings before writing). Wayback prefix stripped from every URL by a 14-digit-timestamp pattern. `target="_blank"` and `rel="noopener"` stripped from every `<a>`. Commented-out `<!-- <li>... -->` blocks for placeholder Blog/Demo/Twitter/Slides/Posters entries skipped. Active (non-commented) hello-world placeholder anchors on entries 6 and 7 dropped (text omitted as well, since the only label is "Paper" and no real URL exists).

## Source-Text Typos & Quirks Preserved Verbatim

Per Phase 1 D-05 (preserve source typos), the following oddities in the wayback source are reproduced byte-for-byte:

1. **Entry 1 (NAACL 2025) venue lacks `<i>` wrap.** Source line 690: `Ngong, I. C., Near, J. P., &amp; Mireshghallah, N.<br>NAACL 2025` — plain text, no italic. The other 6 entries' venues are wrapped in `<i>...</i>`. Preserved as-is.
2. **Entry 2 (SOUPS) venue: missing space before parenthesis.** Source: `Symposium of Usable Privacy and Security(SOUPS 2024)` — note no space between "Security" and "(". Preserved.
3. **Entry 2 (SOUPS) venue: trailing space before `<br>`.** Source: `Feng, Y. <br><i>...</i>` — single trailing space after "Y." before the `<br>`. Preserved.
4. **Entry 3 (OLYMPIA) venue: trailing `&nbsp;&nbsp;` after authors trimmed.** Source line 965: `Ngong, I. C., Gibson, N., &amp; Near, J. P.&nbsp;&nbsp;<br><i>...</i>`. Per the planner's extraction rule 12, the two trailing `&nbsp;` before `<br>` were trimmed (they were trailing whitespace, not embedded). Result: `Ngong, I. C., Gibson, N., &amp; Near, J. P.<br><i>2024 IEEE Conference on Secure and Trustworthy Machine Learning (SaTML)</i>`.
5. **Entry 4 (COVID-19) title: trailing space trimmed.** Source line 1090: `<h2>Different Deep Learning Based Classification Models for COVID-19 CT-Scans and Lesion Segmentation Through the cGAN-UNet Hybrid Method </h2>` — trailing space before `</h2>`. Per the planner's extraction rule 11, leading/trailing whitespace was stripped. Result: title ends with "Method" (no trailing space).
6. **Entry 5 (Prediction Sensitivity) venue: comma-then-space-year format.** Source: `<i>Arxiv, 2022</i>` (atypical — most papers say "ArXiv 2022" without the comma). Preserved.
7. **Entry 6 (Heart Disease) venue: trailing period.** Source: `<i>Springer, Cham, 2021.</i>` — trailing period inside the italic. Preserved.
8. **Entry 7 (Towards auditability) venue: NO `<br>` between authors and venue.** Source line 1482: `IC Ngong, K Maughan, JP Near<i>ACL Arxiv 2020</i>` — note authors and venue are run together with no line break (clearly an Elementor authoring mistake). Preserved verbatim. The CSS will render this as a single line; if the owner wants the line break later, that's a one-character HTML fix that doesn't belong in this verbatim-extraction plan.

## TLDR Curly-Quote Preservation

Each of the 7 TLDR paragraphs begins with `"` (U+201C, left double-quotation mark) and ends with `"` (U+201D, right double-quotation mark). Both bytes preserved exactly from the wayback source. The TLDR for entry 7 also contains an internal pair of ASCII straight quotes around `"group-fair"` — also preserved verbatim. Internal apostrophes (e.g., entry 1's `'bad'` and entry 4's `99.20%.`) reproduce the source's character set (single straight quotes, ASCII period).

## Flagged for Owner (per CONT-09)

Two issues found in the wayback source that the executor surfaces rather than fabricates:

1. **Entry 6 (Heart Disease) — broken Paper URL.** Source line 1428 has `<a href="https://web.archive.org/web/20250625012030/https://ivolinengong.com/hello-world" target="_blank">Paper</a>`. Stripping the wayback prefix yields `https://ivolinengong.com/hello-world`, which is the WordPress default "Hello World" placeholder URL — clearly an authoring mistake. Per critical_constraint #1 of the executor brief ("Drop broken WordPress placeholder anchors — keep the text"), the anchor was dropped and entry 6 has no `.pub-links` row. **Owner action requested:** supply the canonical paper URL (likely `https://link.springer.com/chapter/10.1007/978-3-030-94191-8_39` based on the matching news-entry link on `index.html` line 134 referencing the same paper at the 6th International Conference on Smart City Applications).
2. **Entry 7 (Towards auditability) — broken Paper URL.** Source line 1558 has the same `https://ivolinengong.com/hello-world` placeholder for the labelled "Paper" link. Dropped per the same rule. The "Posters" link on line 1573 is a real Google Docs presentation and was retained. **Owner action requested:** supply the canonical paper URL (the news entry on `index.html` line 130 references the related Arxiv paper at `https://arxiv.org/abs/2202.04504`, but that's actually the Prediction Sensitivity preprint, not this 2020 auditability paper — the 2020 paper is `arXiv:2012.00106` per the wayback citation block on line 1590).

These flags do NOT block this plan's completion — the page renders complete content for both entries (title, authors, venue, TLDR); only the resource-link row is missing for one and reduced for the other. Adding the real URLs later is a 2-line patch.

## Deviations from Plan

### Reported

**1. [Rule 1 — Bug: source data] Dropped two `hello-world` placeholder Paper URLs (entries 6 & 7)**
- **Found during:** Task 2 extraction
- **Issue:** Wayback source has `https://ivolinengong.com/hello-world` (WordPress default placeholder) as the canonical "Paper" link for entries 6 and 7 — these are not real URLs and would 404. Including them would violate the "no broken anchors" critical constraint and would mislead readers.
- **Fix:** Dropped both placeholder anchors. Entry 6 has no `.pub-links` row. Entry 7 retains only the real Posters link. Flagged both in the "Flagged for Owner" section above with the most likely canonical replacements found in the wayback news entries / citation blocks.
- **Files modified:** `publications.html` (the two omissions are in entries 6 and 7)
- **Commit:** `e3509e6`

**2. [Planner-estimation artifact] Line-count gate failed by 2 (88 lines vs ≥90 expected)**
- **Found during:** Task 2 automated verification
- **Issue:** Plan's automated verify gate included `[ "$(wc -l < publications.html)" -ge "90" ]`, annotated by the planner as a "rough lower bound: 7 entries × ~10 lines + ~25 lines of shell". Actual breakdown: 28 lines of shell + heading, 60 lines of 7 entries (averaging 8.6 lines per entry, not the planner's estimated 10), total 88. The 2-line shortfall is purely a planner-estimation artifact — every substantive content-fidelity gate (7 articles, 7 titles, 7 venues, 6 link rows, 7 TLDRs, 7 title-string contains-checks, 3 canonical URL contains-checks, 0 wayback prefixes, 0 target=\"_blank\", 0 scripts, 0 hello-world in non-comments) PASSES.
- **Fix:** None — the markup is correct per the planner's template. Padding the file with cosmetic blank lines to satisfy a heuristic would be anti-pattern (the line count would no longer reflect actual content density). Flagged here for transparency; no rule-1/2/3 trigger because the structural correctness is intact.
- **Files modified:** None
- **Commit:** N/A

### Auto-fixed Issues

None.

## Threat-Model Dispositions Honored

Per the plan's `<threat_model>` STRIDE register:

| Threat | Disposition | Evidence in delivered file |
|--------|-------------|----------------------------|
| T-02-02-01 (spoofing — external paper links) | accept | All 7 outbound `<a href>` URLs point to canonical academic sources (aclanthology, usenix, ieeexplore, proquest, montrealethics, arxiv, github, docs.google) — no user-supplied content, no query-string-driven content. |
| T-02-02-02 (wayback-prefix injection) | mitigate | `grep -c 'web.archive.org' publications.html` returns 0 (verified). |
| T-02-02-03 (tabnabbing) | mitigate | `grep -c 'target="_blank"' publications.html` returns 0 (verified). No `rel="noopener"` either (no need — no `target="_blank"`). |
| T-02-02-04 (information disclosure — TLDR text) | accept | TLDR text is owner-published abstracts from the wayback page; nothing private. |
| T-02-02-05 (repudiation / integrity — verbatim claim) | mitigate | Every title, author, venue, and TLDR copied byte-for-byte from the wayback source. Two cases of missing source data are flagged in "Flagged for Owner" rather than fabricated. |
| T-02-02-06 (DoS — page weight) | accept | 88-line static HTML, zero images, no scripts — first paint is trivial. |
| T-02-02-SC (package install) | n/a | Zero packages installed; this plan touches one HTML file. |

## Phase 1 Invariants Audited (No Regressions)

| Invariant | Status | Evidence |
|-----------|--------|----------|
| No `target="_blank"` anywhere | OK | `grep -c 'target="_blank"' publications.html` = 0 |
| No `<script>` tags | OK | `grep -c '<script' publications.html` = 0 |
| No inline `<style>` | OK | `grep -c '<style' publications.html` = 0 |
| No CDN / external `<link>` | OK | Only `<link rel="stylesheet" href="style.css">` (same-origin) |
| Site-shell consistency with index.html | OK | doctype + html lang + head structure + skip-link + .site-header + .site-nav identical, only the active-marker placement differs (per spec) |
| `<h2>` is the first heading level in `<main>` (no `<h1>`) | OK | `<h2>Publications</h2>` is the only `<h2>` in this file's `<main>`; each pub-entry uses `<p class="pub-title">` not `<h3>` to keep heading hierarchy flat per Phase 1's rule |
| Same-origin nav anchors only | OK | All 4 nav `<a href>` values are bare relative filenames (index.html / talks.html / code.html / writing.html) |

## Verification

All end-of-plan gates from the plan's `<verification>` block return clean:

```
test -f publications.html                                          → OK
[ "$(grep -c '<article class="pub-entry">' publications.html)" = "7" ] → OK
grep -q '<title>Publications — Ivoline Ngong</title>' publications.html → OK
grep -q '<span aria-current="page">Papers</span>' publications.html → OK
! grep -q 'web.archive.org' publications.html                      → OK
! grep -q 'target="_blank"' publications.html                      → OK
! grep -q '<script' publications.html                              → OK
grep -q '<a href="index.html">Main</a>' publications.html          → OK
grep -q '<a href="talks.html">Talks</a>' publications.html         → OK
grep -q '<a href="code.html">Code</a>' publications.html           → OK
grep -q '<a href="writing.html">Writing</a>' publications.html     → OK

VERIFICATION COMPLETE  (zero FAIL lines printed)
```

Task 2's content-fidelity automated gates also all PASS (18/18 substantive checks). The single line-count heuristic (≥ 90) fails at 88 — annotated by the planner as a "rough lower bound" and documented as a planner-estimation artifact in "Deviations from Plan" above.

## Commits

| Hash | Type | Files | Description |
|------|------|-------|-------------|
| `3a6c902` | feat(02-02) | publications.html | Scaffold publications.html shell + nav |
| `e3509e6` | feat(02-02) | publications.html | Add 7 publication entries from wayback source |

## Threat Flags

None. This plan adds one HTML file containing 7 same-page-rendered articles and 11 external `<a href>` links to academic-canonical hosts (aclanthology.org, usenix.org, ieeexplore.ieee.org, github.com, proquest.com, montrealethics.ai, arxiv.org, docs.google.com). No new auth surface, no new schema, no new file-access patterns, no inputs. The trust boundary remains "visitor browser ← static HTML served by GitHub Pages → outbound clicks to third-party academic hosts" — identical in shape to the link surface already in `index.html`'s news list and profile card.

## Known Stubs

None on this page. The two dropped placeholder Paper anchors (entries 6 + 7) are documented as "Flagged for Owner" — they are NOT stubs (a stub would be a fake link that pretends to work; dropping a broken link and rendering only the title+venue+TLDR is the correct Carlini-minimal response per the critical constraint).

## Self-Check: PASSED

Files claimed in this SUMMARY verified to exist on disk and at the expected commits:

- `/Users/ivolinengong/Documents/My_Website/publications.html` — exists, 88 lines.
  - `grep -c '<article class="pub-entry">' publications.html` → 7 ✓
  - `grep -c '<p class="pub-tldr">' publications.html` → 7 ✓
  - `grep -c 'web.archive.org' publications.html` → 0 ✓
  - `grep -c 'target="_blank"' publications.html` → 0 ✓
  - `grep -c 'hello-world' publications.html` → 0 ✓ (in non-commented lines; no comments in the file either)
  - `grep -c '<script' publications.html` → 0 ✓
- Commit `3a6c902` — present in `git log --oneline` on `main`, subject `feat(02-02): scaffold publications.html shell + nav`.
- Commit `e3509e6` — present in `git log --oneline` on `main`, subject `feat(02-02): add 7 publication entries from wayback source`.
- All 11 end-of-plan verification gates PASS (zero FAIL lines).
- 18 of 19 Task 2 acceptance gates PASS; the single line-count heuristic (≥90) is documented as a planner-estimation artifact, not a structural defect.
