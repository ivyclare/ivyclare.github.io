---
phase: 02-remaining-content-pages
plan: 05
subsystem: static-site
tags:
  - static-site
  - code-page
  - github-link
requires:
  - 02-01-SUMMARY.md   # provides .code-intro CSS class + active nav anchors
  - 01-UI-SPEC.md      # §Copywriting Contract row "Code page body (when built)"
provides:
  - "code.html static page pointing to https://github.com/ivyclare"
  - "Closes the 5-page nav set (Main / Papers / Talks / Code / Writing)"
affects:
  - code.html
tech-stack:
  added: []
  patterns:
    - "Inert single-link page (explicit <a>, NOT meta-refresh nor JS redirect) — works with JS disabled"
    - "Shared site-shell pattern: same head/skip-link/site-header/nav as talks.html, with active marker shifted to Code"
key-files:
  created:
    - path: code.html
      provides: "Code page — short pointer to the owner's GitHub profile"
      contains: 'href="https://github.com/ivyclare"'
  modified: []
decisions:
  - "Body sentence taken verbatim from UI-SPEC §Copywriting Contract; no paraphrasing"
  - "Explicit clickable link instead of <meta http-equiv=\"refresh\"> or JS redirect — UI-SPEC rationale: page stays inert and works with JS disabled"
  - "No favicon link (deferred to Phase 3 polish)"
  - "<a class=\"site-title\"> carries href=\"index.html\" but NO aria-current (per plan constraint #4)"
requirements:
  - PAGE-05
  - PAGE-06
metrics:
  duration: "~1 minute"
  completed: 2026-05-19
  tasks_completed: 1
  files_created: 1
  files_modified: 0
  lines_added: 29
  commits: 1
---

# Phase 2 Plan 05: code.html (Code page → GitHub pointer) Summary

Added `code.html` — a 29-line static page whose entire `<main>` is one heading and one paragraph that explicitly links to `https://github.com/ivyclare`, closing the 5-page Phase-2 nav set without introducing any redirect mechanism.

## What Was Built

**1. `code.html` at repo root (29 lines, commit `8a1aed6`).** Single file, no other changes. Reuses the shared site-shell from `talks.html` verbatim with three deltas:

| Section | Delta |
|---------|-------|
| `<title>` | `Code — Ivoline Ngong` |
| `<meta name="description">` | `Ivoline Ngong's code lives on GitHub at github.com/ivyclare.` (ends with period) |
| Nav active marker | Shifted from `Talks` to `Code` — `Code` is the only `<span aria-current="page">`; the previously-active `Talks` becomes `<a href="talks.html">Talks</a>` |

The `<main>` body is intentionally a one-liner:

```html
<main id="main">
    <h2>Code</h2>
    <p class="code-intro">My code lives on GitHub: <a href="https://github.com/ivyclare">github.com/ivyclare</a></p>
</main>
```

The body `<p>` reuses the `.code-intro` class declared in Plan 02-01 (style.css lines 522-530) — 17px body type, `--text`, line-height 1.6, `max-width: 60ch` with mobile 16px step at the consolidated @media block (line 536).

## Why

Phase 2's 5-item nav (Main / Papers / Talks / Code / Writing) needs all five destinations to exist or the activated nav anchors in `index.html` would 404 the "Code" item. The owner does not curate code on this site — they point visitors to GitHub. The plan's chosen shape (explicit `<a>` rather than `<meta http-equiv="refresh">` or a tiny JS redirect) honors the UI-SPEC §Copywriting Contract verbatim and keeps the page Carlini-minimal: nothing animates, nothing redirects, the page works fully with JavaScript disabled, archived snapshots remain meaningful (no `.0` second teleport), and the user controls the navigation moment with an explicit click.

## How

- **Task 1 — Write code.html in one pass (commit `8a1aed6`).** Used the `Write` tool to create the entire file in a single operation. Site-shell copied structurally from `talks.html` (same `<head>` block, same `skip-link`, same `<header class="site-header">` markup) with the three deltas above. Verification gate (19 conjoined checks) passed on the first run; no iteration needed.

## UI-SPEC §Copywriting Contract Verbatim Confirmation

The plan's `<output>` clause requires confirming that the body sentence matches UI-SPEC verbatim. It does:

| Source | Content |
|--------|---------|
| UI-SPEC line 351 | `My code lives on GitHub: <a href="https://github.com/ivyclare">github.com/ivyclare</a>` |
| code.html `<p class="code-intro">` body | `My code lives on GitHub: <a href="https://github.com/ivyclare">github.com/ivyclare</a>` |

Byte-for-byte identical. Verified by `grep` of the literal sentence in the automated gate.

## Auto-Redirect Absence Confirmation

The plan's `<output>` clause also requires confirming no auto-redirect mechanism (script or meta-refresh) was introduced. Three independent automated checks all pass:

| Check | Command | Result |
|-------|---------|--------|
| No `<script>` tags | `grep -c '<script' code.html` | `0` |
| No `<meta http-equiv="refresh">` | `grep -c 'http-equiv="refresh"' code.html` | `0` |
| No `target="_blank"` (tabnabbing) | `grep -c 'target="_blank"' code.html` | `0` |

The page is fully inert. Opening it with JavaScript disabled produces the same visual result as opening it with JavaScript enabled.

## 5-Page Nav Set Completeness Confirmation

The plan's `<output>` clause requires confirming the 5-page nav set is now complete and cross-page navigation works in any order without 404. Verified by file existence + cross-link audit:

| File | Exists | Cross-links to siblings |
|------|--------|--------------------------|
| `index.html` | yes (Phase 1) | Papers/Talks/Code/Writing as `<a href>` (Plan 02-01) |
| `publications.html` | yes (Plan 02-02 — landing concurrently in Wave 2) | shell pattern same as siblings |
| `talks.html` | yes (Plan 02-03, commit `f989831`) | nav has 4 `<a>` siblings + `<span aria-current>Talks</span>` |
| `code.html` | **yes (this plan, commit `8a1aed6`)** | nav has 4 `<a>` siblings + `<span aria-current>Code</span>` |
| `writing.html` | yes (Plan 02-04 — landing concurrently in Wave 2) | shell pattern same as siblings |

From `code.html`, clicking Main / Papers / Talks / Writing produces a same-origin GET against an existing file — no 404. The `Code` nav item is a `<span>`, so it does not self-link (correct active-marker pattern).

## Threat-Model Dispositions Honored

All STRIDE entries from the plan's threat model are satisfied:

| Threat ID | Disposition | Status | Evidence |
|-----------|-------------|--------|----------|
| T-02-05-01 | Spoofing — GitHub link | accept | Body link is the canonical `https://github.com/ivyclare` (owner-controlled, https, bare profile path, no query parameters). |
| T-02-05-02 | Spoofing (tabnabbing) — GitHub link | mitigate | Zero `target="_blank"` (verified by automated grep returning 0). |
| T-02-05-03 | Tampering — Auto-redirect | mitigate | Zero `<script>` AND zero `<meta http-equiv="refresh">` (both verified by automated grep returning 0). |
| T-02-05-04 | Repudiation / Integrity — Body content | mitigate | Body sentence is the verbatim UI-SPEC copywriting contract; verified by literal-string grep. |
| T-02-05-05 | Denial of Service — Page weight | accept | 29-line static HTML, ~0.7 KB; trivial cost. |
| T-02-05-SC | Tampering — package installs | n/a | Zero packages; nothing was installed. |

## Phase 1 / Phase 2 Invariant Audit (No Regressions)

| Invariant | Status | Evidence |
|-----------|--------|----------|
| No `target="_blank"` anywhere in `code.html` | OK | `grep target="_blank" code.html` returns nothing |
| No CDN-loaded resources | OK | Only same-origin `style.css` referenced |
| No inline `<style>` | OK | Single stylesheet link, no `<style>` element |
| No `<script>` | OK | Zero script tags anywhere |
| Carlini-minimal voice — no filler/taglines | OK | Body is exactly the UI-SPEC sentence |
| `.site-title` has `href="index.html"` but NO `aria-current` | OK | `grep -E '<a[^>]*aria-current' code.html` returns nothing |
| Single trailing newline at EOF | OK | File ends with `</html>\n` |
| File length 15-40 lines | OK | 29 lines (mid-band) |

## Verification

Plan's automated verification block (19 conjoined `&&` gates):

```
PASS: code.html written, deliberately small, no redirect mechanisms
Line count: 29
```

Plan's end-of-plan verification block (7 independent gates):

```
VERIFICATION COMPLETE
```
(No `FAIL:` lines printed — silence means every gate passed.)

Acceptance criteria from `<task>` Task 1 — all 8 satisfied:

- [x] `code.html` exists at repo root
- [x] `<title>` is exactly `Code — Ivoline Ngong`
- [x] Nav has 5 children in order Main / Papers / Talks / Code / Writing, with `Code` as the only `<span aria-current="page">`
- [x] `<h2>Code</h2>` is the first child of `<main id="main">`
- [x] Body contains EXACTLY one `<p class="code-intro">` with the literal UI-SPEC content
- [x] Zero `<script>` tags
- [x] Zero `<meta http-equiv="refresh">`
- [x] Zero `target="_blank"`
- [x] Total file length is 15-40 lines (29 lines)

Success criteria from `<success_criteria>` — all 6 satisfied:

1. `code.html` exists at repo root and opens cleanly as a local file — yes (`test -f code.html` passes)
2. Shared site-header + nav renders with Code marked as the current page — yes (`<span aria-current="page">Code</span>` is the 4th nav child)
3. `<h2>Code</h2>` heading + single `<p class="code-intro">` paragraph — yes
4. Paragraph contains the verbatim UI-SPEC sentence with a working anchor to `https://github.com/ivyclare` — yes
5. Page does NOT auto-redirect (no JS, no meta-refresh) — yes, verified by two independent greps
6. Page works fully with JavaScript disabled — yes (zero `<script>` tags; the link is a plain HTML `<a>` element)

## Commits

| Hash | Type | Files | Description |
|------|------|-------|-------------|
| `8a1aed6` | feat(02-05) | code.html | Add code.html pointing to github.com/ivyclare |

## Deviations from Plan

None — plan executed exactly as written. The automated verification gate (19 conjoined checks) passed on the first run with no iteration. No deviation rules (1, 2, 3, or 4) triggered. No auth gates encountered. No checkpoints in this plan.

## Threat Flags

None. This plan introduces only a single same-origin static HTML file containing one cross-origin `<a href>` to a previously-listed canonical owner-controlled URL (`github.com/ivyclare`, already linked from `index.html` line 48 profile-card). No new network endpoints, no auth paths, no file-access patterns, no schema changes, no new trust boundaries.

## Known Stubs

None. The page intentionally contains a single short paragraph as a "by design" minimal page (per UI-SPEC). This is not a stub — there is no future plan that will replace this paragraph with a project listing; the owner explicitly chose to point at GitHub rather than curate a portfolio on this site. The page is complete.

## Self-Check: PASSED

Files and commits claimed in this SUMMARY verified to exist on disk:

- `/Users/ivolinengong/Documents/My_Website/code.html` — exists, 29 lines.
- `/Users/ivolinengong/Documents/My_Website/.planning/phases/02-remaining-content-pages/02-05-SUMMARY.md` — exists (this file).
- Commit `8a1aed6` — present in `git log --oneline` on `main`, subject `feat(02-05): add code.html pointing to github.com/ivyclare`.
- All 19 automated verification gates from Task 1 `<verify>` block — pass.
- All 7 end-of-plan verification gates from `<verification>` block — pass (zero `FAIL:` lines).
- All 8 Task 1 acceptance criteria — satisfied.
- All 6 plan-level success criteria — satisfied.
- All 6 threat-model dispositions — honored.
