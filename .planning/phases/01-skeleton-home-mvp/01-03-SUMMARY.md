---
phase: 01-skeleton-home-mvp
plan: 03
subsystem: static-site/home
status: complete
tags:
  - static-site
  - content-extraction
  - home-page
requirements_completed:
  - CONT-01
  - CONT-02
  - CONT-03
  - CONT-04
  - PAGE-01
  - PAGE-07
  - PAGE-08
  - INFRA-03
dependency_graph:
  requires:
    - 01-01 (assets: profile_pic.png, CV PDF)
    - 01-02 (style.css selector contract)
    - 01-UI-SPEC.md (DOM order, copywriting, contact strip)
    - reference/wayback/Home - Ivoline Ngong.html (primary content source)
  provides:
    - index.html (the Home page — site entry point)
  affects:
    - 01-04 (visual verification plan — opens this file in browser)
    - Phase 2 plans (Papers/Talks/Code/Writing pages will swap nav placeholders for real links)
tech-stack:
  added:
    - hand-written HTML5 (no templating, no build step, no JS)
  patterns:
    - semantic HTML5 sections (nav, main, header, section)
    - aria-current="page" + aria-labelledby for a11y
    - tabindex="0" on scrollable news container for keyboard a11y
    - data-todo="phase-2" markers on inactive nav spans
    - explicit img width/height to prevent CLS
    - mailto: and same-tab external links (no target="_blank" → no tabnabbing risk)
key-files:
  created:
    - index.html
  modified: []
decisions:
  - linkedin-recovery: Used acceptable-fallback (rule 6) — grepped wayback HTML and found a LinkedIn anchor at line 1344 (`https://www.linkedin.com/in/iviolinengong`). Used the canonical URL. **Owner flag:** the handle "iviolinengong" looks like a typo of "ivolinengong" (matches the email/scholar/site pattern); owner should verify and we'll correct in a one-line follow-up edit.
  - nav-placeholders: Rendered Papers/Talks/Code/Writing as `<span data-todo="phase-2">` (not `<a>`) because their HTML files don't exist yet in Phase 1; preserves visual completeness of nav without producing 404s. Phase 2 plans will swap each span into `<a href="publications.html">`/etc.
  - header-wrapper: Used `<div class="header-text">` to group H1+role+contact for flex sibling layout against the headshot. Plan 02's CSS treats `<header>` as a flex row, so this scoping mirrors that contract.
  - bio-broken-links: Removed 3 WordPress placeholder anchors (`http://p/`, `http://ss/`, `http://d/` with `data-wplink-url-error="true"`) per extraction protocol rule 3. Visible text `privacy`, `security`, `safety` preserved inline (wrapping `<b>` retained from source).
  - news-broken-links: 2 May 2025 entries had `<a href='ivolinengong.com/'>` self-references that don't link to anything specific. Per rule 3, dropped the `<a>` wrappers but retained the paper-title text inside `<b>` tags.
  - typo-preservation: Preserved verbatim source typos (`Decemeber`, `Febraury`, `comittee`, `Initializaiton`, `Organzing`) per extraction protocol rule 9. Note these for Phase 3 owner-review opportunity.
  - role-line: Used UI-SPEC override `PhD Candidate · University of Vermont` (clean form) instead of the wayback "PhD Candidate, UVM ivolinengong@gmail.com" artifact (line 703 — wayback rendering collision).
metrics:
  duration: ~21 minutes
  completed: 2026-05-19
  tasks_completed: 2
  files_created: 1
  files_modified: 0
  line_count: 142
---

# Phase 1 Plan 01-03: index.html Summary

Hand-written 142-line HTML5 Home page implementing the UI-SPEC DOM contract: 5-item nav · header with headshot + bio + contact strip · 6-paragraph bio extracted verbatim from wayback · scrollable .news-list container with all 42 news entries (Aug 2021 → May 2025, reverse-chronological). Pure HTML+CSS, zero scripts, zero CDN, zero Wayback artifacts.

## What was built

`/Users/ivolinengong/Documents/My_Website/index.html` — 142 lines, one file, opens cleanly via `file://`.

### Structural breakdown

| DOM | Lines | Content |
|-----|-------|---------|
| `<head>` | 3-9 | charset, viewport, `<title>Ivoline Ngong</title>`, description, stylesheet link |
| skip link | 11 | first child of `<body>`, `href="#main"` |
| `<nav aria-label="Site">` | 13-19 | 5 items: 1 active `<span aria-current="page">Main</span>`, 4 phase-2 placeholder spans |
| `<header>` | 22-35 | `<img class="headshot">` + `<div class="header-text">` (h1, role, contact strip) |
| `<section id="about">` | 37-49 | 6 bio paragraphs |
| `<section id="news">` | 51-138 | `<h2 id="news-heading">What's New?</h2>` + scrollable `.news-list` with 42 `.news-entry` blocks |

## Content extraction summary

### Bio (6 paragraphs)

Extracted verbatim from wayback lines 682 + 686. Multi-paragraph: research focus + crossroads (privacy/security/safety) → fundamental questions paragraph → multi-level trustworthiness (MPC/DP/contextual privacy) → IBM Research mentorship paragraph → Fun fact (three continents) → Interested in collaborating closing line.

Inline links resolved (all Wayback prefixes stripped):
- Joe Near: `https://www.uvm.edu/~jnear/`
- Karthikeyan Natesan Ramamurthy: `https://research.ibm.com/people/karthikeyan-natesan-ramamurthy`
- Hao Wang: `https://haowang94.github.io/`
- Amit Dhurandhar: `https://research.ibm.com/people/amit-dhurandhar`

Broken-link wrappers removed (3 WordPress placeholder anchors with `data-wplink-url-error="true"`):
- `<a href="http://p/">privacy</a>` → `privacy` (text only)
- `<a href="http://ss/">security</a>` → `security` (text only)
- `<a href="http://d/">safety</a>` → `safety` (text only)

### Contact strip (6 links)

| Item | Resolved href | Source |
|------|---------------|--------|
| Email | `mailto:ivolinengong@gmail.com` | UI-SPEC §Contact strip + wayback line 703 |
| Twitter | `https://twitter.com/IvolineNgong` | wayback line 709, Wayback-prefix stripped |
| Scholar | `https://scholar.google.com/citations?user=DzEo62wAAAAJ&amp;hl=en` | wayback line 715, prefix stripped, `&amp;` preserved |
| GitHub | `https://github.com/ivyclare` | wayback line 721, prefix stripped |
| LinkedIn | `https://www.linkedin.com/in/iviolinengong` | wayback line 1344 (footer), prefix stripped — **see Flagged for Owner below** |
| CV | `assets/Ivoline_Ngong_CV_August_2025.pdf` | UI-SPEC override (decision D-08); local relative path |

### News entries (42 total)

First entry: **May 2025:** Our paper *Differentially Private Learning Needs Better Model Initialization and Self-Distillation* accepted at TPDP 2025
Last entry: **Aug 2021:** Started PhD at the University of Vermont

Span: Aug 2021 → May 2025 (matches UI-SPEC §Why scrollable "~30+ entries spanning 2021–2025"; actual is 42).

Order: reverse-chronological, matching source order (already in reverse-chronological in wayback).

#### Inline news-entry links extracted (Wayback-prefix stripped)

| # | Date | Canonical URL | Label |
|---|------|--------------|-------|
| 3 | April 2025 | https://tpdp.journalprivacyconfidentiality.org/2025/ | TPDP 2025 |
| 4 | March 2025 | https://opendp.org/blog/reflections-differential-privacy-beyond-algorithms-workshop | Reflections from the Differential Privacy Beyond Algorithms Workshop |
| 5 | February 2025 | https://facctconference.org/ | ACM FAccT 2025 |
| 6 | January 2025 | https://www.moveworks.com/ | Moveworks |
| 7 | January 2025 | https://arxiv.org/pdf/2410.17566 | Differentially Private Learning Needs Better Model Initialization and Self-Distillation |
| 8 | Decemeber 2024 | https://arxiv.org/pdf/2412.16825 | SoK: Usability Studies in Differential Privacy |
| 10 | October 2024 | https://arxiv.org/pdf/2410.17566 | Differentially Private Learning Needs Better Model Initialization and Self-Distillation |
| 11 | August 2024 | https://sites.harvard.edu/opendp/parallel-breakout-descriptors/ | DP Beyond Algorithms workshop |
| 12 | August 2024 | https://www.usenix.org/system/files/soups2024-ngong.pdf | Evaluating the Usability of Differential Privacy Tools with Data Practitioners |
| 13 | July 2024 | https://nips.cc/ | NeurIPS 2024 |
| 14 | June 2024 | https://www.usenix.org/system/files/soups2024-ngong.pdf | Evaluating the Usability of Differential Privacy Tools with Data Practitioners |
| 16 | May 2024 | https://tpdp.journalprivacyconfidentiality.org/2024/ | TPDP 2024 - Theory and Practice of Differential Privacy |
| 17 | April 2024 | https://arxiv.org/abs/2302.10084 | OLYMPIA: A Simulation Framework for Evaluating the Concrete Scalability of Secure Aggregation Protocols |
| 18 | March 2024 | https://blog.openmined.org/openmined-featured-contributor-march-2024/ | Openmined contributor of the month |
| 19 | December 2023 | https://ppai-workshop.github.io/ | The Fifth AAAI Workshop on Privacy-Preserving Artificial Intelligence (PPAI-24) |
| 20 | November 2023 | https://www.tmlt.io/ + https://arxiv.org/abs/2309.13506 | Tumult Labs · Evaluating the Usability of Differential Privacy Tools with Data Practitioners |
| 21 | September 2023 | https://arxiv.org/abs/2309.13506 | Evaluating the Usability of Differential Privacy Tools with Data Practitioners |
| 23 | August 2023 | https://tpdp.journalprivacyconfidentiality.org/2023/ | TPDP 2023 |
| 24 | July 2023 | https://nips.cc/ | NeurIPS 2023 |
| 25 | July 2023 | https://blog.openmined.org/ai-audit-part-1/ | How To Audit An AI Model Owned by Someone else (Part 1) |
| 26 | June 2023 | https://blog.openmined.org/openmineds-r4q2-padawan-program-graduates/ | Padawan Program |
| 27 | May 2023 | https://genlaw.github.io/index.html | GenLaw workshop |
| 28 | Febraury 2023 | https://arxiv.org/abs/2302.10084 | OLYMPIA: A Simulation Framework for Evaluating the Concrete Scalability of Secure Aggregation Protocols |
| 29 | Febraury 2023 | https://www.proquest.com/openview/dd9a6b095d7a8bc3f3d331f47277dfab/1?pq-origsite=gscholar&cbl=2069443 | Different Deep Learning Based Classification Models for COVID-19 CT-Scans and Lesion Segmentation Through the cGAN-UNet Hybrid Method |
| 30 | Febraury 2023 | https://factored.ai/learning-fest/ | Factored Learning Fest |
| 31 | December 2022 | https://wimlworkshop.org/2022-wiml-workshop/ | Women in Machine Learning Workshop (WiML) |
| 32 | August 2022 | https://www.microsoft.com/en-us/research/academic-program/rl-open-source-fest/alumni/ | Compiler optimization for Reinforcement Learning |
| 33 | June & August 2022 | https://www.oxfordml.school/ | ML Oxford Summer ML |
| 34 | June 2022 | https://bostondataprivacy.github.io/summerschool/ | Differential Privacy summer school |
| 35 | June 2022 | https://montrealethics.ai/prediction-sensitivity-continual-audit-of-counterfactual-fairness-in-deployed-classifiers/ | featured |
| 36 | June 2022 | https://www.openmined.org/ | Openmined |
| 37 | May 2022 | https://www.youtube.com/watch?v=9zFhJ5vjBtw | talk on Privacy and Fairness in AI |
| 38 | Feb 2022 | https://arxiv.org/abs/2202.04504 | Prediction Sensitivity: Continual Audit of Counterfactual Fairness in Deployed Classifiers |
| 40 | Oct 2021 | https://link.springer.com/chapter/10.1007/978-3-030-94191-8_39 | Feature Extraction Methods for Predicting the Prevalence of Heart Disease |
| 41 | Sept 2021 | https://research.google/outreach/csrmp/ | Google's CS Research Mentorship Program |

(Entries 1, 2, 9, 15, 22, 39, 42 have no inline links — text-only news items.)

#### Italics preserved

Entry 35 (June 2022) — `<i>'Prediction Sensitivity: Continual Audit of Counterfactual Fairness in Deployed Classifiers'</i>` preserved verbatim per UI-SPEC §Typography (italics allowed for inline paper titles in news).

#### Special characters preserved

- Smart quotes (`'`, `'`) in bio paragraph 2 (questions), bio paragraph 5 (Fun fact "I'm" / "It's"), and Sept 2021 entry ("Google's").
- Em dash (`—`) in `<meta description>` and bio paragraph 5 ("the journey started").
- Emoji 🔥 in second May 2025 entry.
- `&amp;` preserved in scholar URL and proquest URL (HTML escaping).

## Verification gates — all passed

| Gate | Required | Actual | Status |
|------|----------|--------|--------|
| `test -f index.html` | exists | exists | PASS |
| `wc -l index.html` | ≥ 80 | 142 | PASS |
| `grep -i '<!doctype html>'` | present | present | PASS |
| `<title>Ivoline Ngong</title>` | exact | exact | PASS |
| `href="style.css"` | present | present | PASS |
| `src="assets/profile_pic.png"` | present | present | PASS |
| `href="assets/Ivoline_Ngong_CV_August_2025.pdf"` | present | present | PASS |
| `class="skip-link"` | present | present | PASS |
| `aria-current="page"` | present | present | PASS |
| `mailto:ivolinengong@gmail.com` | present | present | PASS |
| `web.archive.org` substrings | 0 | 0 | PASS |
| `data-wplink-url-error` substrings | 0 | 0 | PASS |
| `<script` tags | 0 | 0 | PASS |
| CDN refs (`googleapis|gstatic|jsdelivr|unpkg|cdn`) in `href`/`src` | 0 | 0 | PASS |
| `class="news-entry"` count | ≥ 15 | 42 | PASS |
| `class="news-list"` | present | present | PASS |
| `tabindex="0"` | present | present | PASS |
| `<h2 id="news-heading">What's New?</h2>` | present | present | PASS |
| Bio `<p>` count | ≥ 3 | 6 | PASS |
| Nav items | 5 (1 active + 4 phase-2 spans) | 5 | PASS |
| `target="_blank"` co-occurring without `rel="noopener noreferrer"` | 0 | 0 (no target="_blank" anywhere) | PASS |
| `assets/profile_pic.png` resolves on disk | yes | yes | PASS |
| `assets/Ivoline_Ngong_CV_August_2025.pdf` resolves on disk | yes | yes | PASS |

## Acceptance criteria — all met

### Task 1 (scaffolding + bio + contact strip)
- [x] index.html exists at repo root
- [x] Doctype `<!doctype html>` (lowercase HTML5)
- [x] `<title>` exactly `Ivoline Ngong`
- [x] `<meta name="description">` matches UI-SPEC §Copywriting
- [x] `<link rel="stylesheet" href="style.css">` in head
- [x] Skip link first body child, points to `#main`
- [x] Nav has 5 items in order Main · Papers · Talks · Code · Writing
- [x] `<img class="headshot" src="assets/profile_pic.png" alt="Portrait of Ivoline Ngong">`
- [x] `<h1>Ivoline Ngong</h1>` + `<p class="role">PhD Candidate · University of Vermont</p>`
- [x] Contact strip: email (mailto), Twitter, Scholar, GitHub, LinkedIn, CV
- [x] `<section id="about">` with ≥ 3 bio paragraphs (actual: 6)
- [x] Zero `web.archive.org` occurrences
- [x] Zero `<script>` tags

### Task 2 (news entries)
- [x] ≥ 15 `.news-entry` blocks (actual: 42)
- [x] Each entry has one `.news-date` span + one `.news-body` span
- [x] Zero `web.archive.org` substrings
- [x] Zero `data-wplink-url-error` substrings
- [x] `<div class="news-list" tabindex="0" aria-label="News updates, scrollable list">` wraps every entry
- [x] `<h2 id="news-heading">What's New?</h2>` heading
- [x] Italics preserved for paper-title in June 2022 entry

## Threat model — all dispositions honored

| Threat ID | Disposition | Status |
|-----------|-------------|--------|
| T-01-03-01 | accept | Links go to canonical public URLs |
| T-01-03-02 | **mitigate** | Verified: zero `web.archive.org` substrings (Wayback-prefix-strip rule enforced) |
| T-01-03-03 | accept | No state |
| T-01-03-04 | accept | Owner-chosen email exposure |
| T-01-03-05 | **mitigate** | Verified: zero `target="_blank"` anywhere in document (no tabnabbing surface). UI-SPEC §Contact strip "opens in same tab" honored. |
| T-01-03-06 | accept (phase) | Profile pic is 2.8MB — flagged below |
| T-01-03-07 | accept | No auth |
| T-01-03-08 | accept | No package installs |

## Flagged for Owner Review

1. **LinkedIn handle (potential typo):** Recovered URL is `https://www.linkedin.com/in/iviolinengong` from wayback line 1344. Note the spelling `iviolinengong` (extra "i") — does not match the email/scholar/site pattern (`ivolinengong`). May be a wayback OCR artifact or a real typo in the original site footer. **Action:** Owner please confirm the correct LinkedIn handle; one-line edit on `index.html` line 31 will swap if needed.

2. **Profile photo file size:** `assets/profile_pic.png` is 2.8MB. UI-SPEC §Profile Photo specifies 120×120 desktop display, so the image is wildly oversized for its rendered footprint. T-01-03-06 documents this is accepted for Phase 1 (lazy+async loading mitigates worst-case) but **Phase 3 QUAL-06 should optimize** — target ≤100KB after compression (PNG → resized PNG or WebP).

3. **Source-text typos preserved verbatim:** Per extraction-protocol rule 9, the following typos from the wayback source are preserved unchanged:
   - `Decemeber 2024:` (entry 8) — should be "December"
   - `Febraury 2023:` (entries 28, 29, 30) — should be "February"
   - `comittee` (entries 3, 5, 16, 19) — should be "committee"
   - `commitee` (entries 14, 16) — should be "committee"
   - `Initializaiton` (entry 6, in quoted talk title) — should be "Initialization"
   - `Co-organzing` (entry 39) — should be "Co-organizing"
   - "PhD Candidate, UVM ivolinengong@gmail.com" wayback artifact (line 703) was **NOT** preserved — UI-SPEC override used cleaner form "PhD Candidate · University of Vermont".
   **Action:** If owner wants the typos corrected at content-fidelity expense, add a one-line typo-fix plan in Phase 3.

4. **Self-referential broken links in news entries 1 & 2:** Both top May 2025 entries had `<a href="ivolinengong.com/">paper-title</a>` in source — pointing at the homepage itself, not at the actual paper PDF. Anchors dropped, text retained. **Action:** Owner may want to wire these to the real paper URLs (likely the same arxiv link as January 2025 entry 7: `https://arxiv.org/pdf/2410.17566`); not done here to avoid fabrication.

## Deviations from Plan

None — plan executed exactly as written. The decision points the plan explicitly opened (LinkedIn handling, header-text wrapper choice, news-section stub-vs-inline) are documented under `decisions` in the frontmatter.

The four flagged-for-owner items above are flags per CONT-09 spirit, not deviations from the plan's spec.

## Threat Flags

None. Nothing in `index.html` introduces a new security surface beyond what is already enumerated in the plan's `<threat_model>`. All outbound links are canonical public URLs (no Wayback proxies, no shorteners). No JavaScript, no third-party iframes, no inline data URIs. The `mailto:` is the only side-channel and is owner-intentional.

## Files

### Created
- `index.html` (142 lines)

### Modified
- (none)

## Commits

| Hash | Type | Description |
|------|------|-------------|
| `a1c12a3` | feat(01-03) | add index.html scaffolding with header, bio, contact strip |
| `e1ba3d4` | feat(01-03) | populate scrollable news list with 42 entries from wayback |

## Self-Check: PASSED

Verified post-commit:
- `index.html` exists at `/Users/ivolinengong/Documents/My_Website/index.html` (142 lines)
- `01-03-SUMMARY.md` exists at `/Users/ivolinengong/Documents/My_Website/.planning/phases/01-skeleton-home-mvp/01-03-SUMMARY.md`
- Commit `a1c12a3` (Task 1) exists in `git log --oneline --all` — confirmed
- Commit `e1ba3d4` (Task 2) exists in `git log --oneline --all` — confirmed
- All 22 end-of-plan verification gates re-ran and PASS (file gates, no-wayback, no-script, no-CDN, news-count ≥ 15, asset resolution, target=_blank tabnabbing absent)
