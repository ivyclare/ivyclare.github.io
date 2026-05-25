# ivolinengong.com — Academic Personal Website Rebuild

## What This Is

A faithful, fully-static rebuild of `ivolinengong.com` — the personal academic
site of Ivoline Ngong (PhD candidate, University of Vermont; research scientist
at OpenMined). The original ran on WordPress on suspended shared hosting with
no backup; this rebuild lives on GitHub Pages from the `ivyclare.github.io`
repo and serves at the custom domain `ivolinengong.com`. Content is recovered
verbatim from Wayback Machine HTML snapshots stored in `reference/`.

## Core Value

**Faithful, durable recreation of the original site's content on infrastructure
that cannot be taken down.** If the site reads true to the original and stays
online for free with zero maintenance, this project succeeded.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Home page (`index.html`) reproduces the original landing page: header, bio, scrollable news/updates timeline, and primary links (including a working CV PDF link)
- [ ] Publications page (`publications.html`) reproduces the original publications listing with venues and links
- [ ] Talks page (`talks.html`) reproduces the original talks listing
- [ ] Writing page (`writing.html`) reproduces the original writing/blog listing
- [ ] Code page (`code.html`) is a short page that redirects (or directly links) to the owner's GitHub profile
- [ ] All content is extracted verbatim from `reference/wayback/` HTML — no fabricated entries
- [ ] Small profile photo (`profile_pic.png`) appears near the header/bio
- [ ] Teal accent color applied consistently (links, headings, dividers); defined once as `--accent` CSS custom property; passes WCAG AA contrast on white background
- [ ] Single shared `style.css` drives all pages; visual style follows Carlini-site minimalism (single-column, content-first, no decorative clutter)
- [ ] Responsive: usable and readable on a ~375px-wide mobile viewport
- [ ] All Wayback prefixes stripped from recovered links; no internal or external 404s
- [ ] `CNAME` file at repo root contains exactly `ivolinengong.com`
- [ ] Repository is structured so GitHub Pages serves it as-is with no build step (no Jekyll config required to render)
- [ ] `index.html` opens correctly as a local file with no console errors
- [ ] `README.md` documents how to edit and rebuild the site

### Out of Scope

- **WordPress / CMS / admin dashboard** — original was killed by dynamic resource usage; static-only is the whole point
- **Server-side code, PHP, database, server build step** — same reason as above
- **Externally-loaded webfonts or framework CDNs that block rendering** — must work fully self-contained
- **Ad networks, cookie/fingerprint-based analytics, PII collection** — privacy and minimalism. (Refined 2026-05-25: lightweight no-cookie no-PII analytics like GoatCounter are now in scope — they don't violate the privacy intent and the owner wants minimal visit telemetry to see which posts get traction.)
- **Backend-powered contact form** — `mailto:` link is sufficient
- **Visual redesign beyond "teal, Carlini-style"** — goal is faithful recreation, not reinvention
- **Jekyll / static site generators** — committed plain HTML keeps the repo serveable with zero configuration
- **DNS configuration, hosting setup, HTTPS enforcement** — owner handles these manually post-build (documented in PROJECT-BRIEF.md §10)

## Context

- **Why this project exists:** Original site ran on WordPress on Namecheap shared hosting. Account was suspended for CPU/memory resource abuse, then canceled. No backup exists. Content was partially recovered from the Internet Archive (Wayback Machine, snapshot dated 2025-06-17) and from saved HTML files.
- **Source material location:**
  - `reference/wayback/` — 4 saved HTML pages (Home, Publications, Talks, Writing) plus their `_files/` asset directories. These are the authoritative record of original content and structure.
  - `reference/screenshot/` — 11 PNG screenshots covering the main page, papers, talks, and writing for visual layout/color reference.
  - `profile_pic.png` (repo root) — owner-provided headshot.
  - `Ivoline_Ngong_CV_August_2025.pdf` (repo root) — owner-provided CV; to be moved into `assets/` and linked from the Home page's CV link.
- **Visual reference:** Nicholas Carlini's site (nicholas.carlini.com) — single-column, content-first, minimal chrome, loads instantly.
- **Author identity:** Ivoline Ngong — PhD candidate in Computer Science at the University of Vermont, research scientist at OpenMined, advised by Joe Near. Research at the intersection of privacy, security, safety, and AI.
- **GitHub handle:** `ivyclare`. Target repo: `ivyclare.github.io` (user-site repo, served at root).
- **PROJECT-BRIEF.md at repo root is the canonical written brief** — treat it as source of truth for goals, constraints, and the post-build manual checklist.

## Constraints

- **Tech stack**: Plain HTML + CSS + at most a tiny amount of vanilla JavaScript — no frameworks, no build step, no server-side code. Reason: dynamic resource usage killed the previous site; static is the architectural mitigation.
- **Hosting**: Must work on GitHub Pages with zero configuration. The repo must serve as-is — no Jekyll config, no Actions workflow required to render. Reason: simplest possible operational footprint.
- **Domain**: Site must serve at `ivolinengong.com`. CNAME file at repo root contains exactly that string.
- **Dependencies**: No CDN-loaded frameworks required for page render. Self-host fonts (or use system font stack). Reason: independence from external services that can break or disappear.
- **Repo**: User-site repo `ivyclare.github.io` on GitHub user `ivyclare`. Served at the domain root.
- **Performance & accessibility**: Semantic HTML, mobile-responsive, fast first paint, WCAG AA contrast on the teal accent. No heavy images.
- **Content fidelity**: Extract all biographical/news/publications/talks/writing content verbatim from `reference/wayback/` HTML. Do NOT invent or paraphrase content. If a section is incomplete in references, flag it rather than fabricating.

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Plain HTML over Jekyll | Brief prefers simplest possible setup; static HTML serves directly on GitHub Pages with no build configuration. Decided 2026-05-19 in questioning. | — Pending |
| Multi-page (5 pages) instead of single long page | Wayback archive captured 4 separate pages (Home, Publications, Talks, Writing) and owner wants a 5th Code page redirecting to GitHub. Faithfulness wins over Carlini-style consolidation. Decided 2026-05-19 in questioning. | — Pending |
| Repo `ivyclare.github.io` (user-site repo at root) | Simplest GitHub Pages setup — no project Pages config, no path handling. Decided 2026-05-19 in questioning. | — Pending |
| Include small profile photo near header | Common for academic personal sites; owner provided `profile_pic.png` and prefers showing it. Departs slightly from Carlini's text-only style. Decided 2026-05-19 in questioning. | — Pending |
| Teal as primary accent color (single `--accent` custom property) | Specified in brief; pick a specific accessible teal hex value that passes WCAG AA against white. Decided in PROJECT-BRIEF.md §5. | — Pending |
| Code page redirects/links to GitHub profile | Owner's preference — they don't host curated code on this site, just point visitors to their GitHub. Decided 2026-05-19 in questioning. | — Pending |
| News section renders inside a fixed-height scrollable container, not as a long inline dump | Owner clarified the original site behavior — the long ~2021–2025 entry list should stay browsable without dominating the Home page. Decided 2026-05-19 mid-Phase-1 setup. | — Pending |
| CV is a real PDF file (`Ivoline_Ngong_CV_August_2025.pdf` at repo root, will move to `assets/`); CV link opens the PDF | Owner provided the file and prefers a direct PDF link over a CV page or external service. Decided 2026-05-19 mid-Phase-1 setup. | — Pending |
| Privacy-respecting analytics added (GoatCounter at `ivolinengong.goatcounter.com`) | Owner wants visit telemetry per page to see which new posts get read. Refines PROJECT-BRIEF.md §9 "no analytics" line: the original intent was no PII/cookie-tracking; GoatCounter has neither so it doesn't violate that intent. Snippet added to all 7 HTML pages including `posts/_template.html` so future posts inherit it. Decided 2026-05-25 after site went live. | ✓ Live |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd:complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-19 after adding scrollable-news + CV-PDF refinements*
