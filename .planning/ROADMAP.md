# Roadmap: ivolinengong.com — Academic Personal Website Rebuild

## Overview

A faithful, fully-static rebuild of `ivolinengong.com` delivered in three demoable slices. Phase 1 stands up the repo skeleton, the shared visual foundation (teal accent, Carlini-style minimalism), and a fully working Home page extracted verbatim from the Wayback snapshot — by the end of Phase 1 the site already opens locally and would deploy. Phase 2 fills in the four remaining content pages (Publications, Talks, Writing, Code) and wires them together with shared navigation. Phase 3 hardens the site for ship: responsive checks at 375px, WCAG AA contrast verification, link-integrity sweep (Wayback prefixes stripped, no 404s), semantic HTML pass, and the README documenting how to edit and rebuild.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (1.1, 1.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [x] **Phase 1: Skeleton + Home MVP** - Repo scaffolding, shared CSS foundation with teal `--accent`, and a working Home page extracted verbatim from the Wayback reference
- [x] **Phase 2: Remaining Content Pages** - Publications, Talks, Writing, and Code pages built on the shared style, wired together with a consistent navigation
- [ ] **Phase 3: Polish & Verification** - Responsive checks, WCAG AA contrast verification, link-integrity sweep, semantic HTML pass, and README documentation

## Phase Details

### Phase 1: Skeleton + Home MVP
**Goal:** A visitor opening `index.html` locally sees the original site's Home page — header, bio, scrollable news timeline, contact links (including a working CV PDF link), and profile photo — styled in Carlini-style minimalism with the teal accent, ready to deploy on GitHub Pages as-is.
**Mode:** mvp
**Depends on:** Nothing (first phase)
**Requirements:** CONT-01, CONT-02, CONT-03, CONT-04, PAGE-01, PAGE-07, PAGE-08, DES-01, DES-02, DES-03, DES-05, DES-06, DES-07, INFRA-01, INFRA-02, INFRA-03, INFRA-04, INFRA-05, INFRA-06
**Success Criteria** (what must be TRUE):
  1. Opening `index.html` locally renders the owner's name, bio, news timeline, contact links, and profile photo — all extracted verbatim from `reference/wayback/Home - Ivoline Ngong.html`
  2. The News section renders inside a fixed-height scrollable container — entries scroll internally rather than dumping all ~2021–2025 entries inline down the page
  3. The CV link in the Home page links to `assets/Ivoline_Ngong_CV_August_2025.pdf` and opens that PDF when clicked
  4. The page is styled by a single shared `style.css` that defines a teal `--accent` CSS custom property, applied consistently to links and headings on a white/near-white background with neutral dark gray body text
  5. The repository root contains `CNAME` (with exactly `ivolinengong.com`), `index.html`, `style.css`, `assets/` (with `profile_pic.png` and `Ivoline_Ngong_CV_August_2025.pdf` moved in), and the existing `reference/` directory — no build step, no Jekyll config, no CDN fonts or frameworks needed to render
  6. The visual feel matches the Carlini-style spirit: single column, content-first, no decorative chrome, fonts served from a system stack or self-hosted


**Plans:** 4/4 plans executed

Plans:
- [x] 01-01-PLAN.md — Repo scaffolding: CNAME, .gitignore, assets/ directory, move profile photo + CV PDF into assets/
- [x] 01-02-PLAN.md — Shared style.css: tokens, base layout, typography, headshot, scrollable news, contact strip, links
- [x] 01-03-PLAN.md — index.html: verbatim bio + scrollable news + contact strip + 5-item nav with Phase 2 placeholders
- [x] 01-04-PLAN.md — End-of-phase verification: 15/15 automated checks passed; owner approved the Carlini-style restructure and the news section formatting

**Owner approval (2026-05-19):** Phase 1 visual deliverable accepted. Owner-driven refinements applied (teal site title, teal news dates, hairline divider between news entries, two-column bio + profile card, serif typography, small-caps nav/site-title/headings).
**UI hint:** yes

 
### Phase 2: Remaining Content Pages
**Goal:** A visitor can navigate from the Home page to Publications, Talks, Writing, and Code via a shared navigation present on every page, and each destination page displays its content extracted verbatim from the matching Wayback reference.
**Mode:** mvp
**Depends on:** Phase 1
**Requirements:** CONT-05, CONT-06, CONT-07, PAGE-02, PAGE-03, PAGE-04, PAGE-05, PAGE-06
**Success Criteria** (what must be TRUE):
  1. `publications.html`, `talks.html`, and `writing.html` each render the corresponding listing (papers with venues and links / talks / writing entries) extracted verbatim from their Wayback reference HTML
  2. `code.html` exists as a short page that points or redirects to the owner's GitHub profile (`github.com/ivyclare`)
  3. A consistent navigation block on every one of the 5 pages links to all 5 pages, so a visitor can move between Home, Publications, Talks, Writing, and Code without using the back button
  4. All five pages share the same `style.css` from Phase 1 — no per-page stylesheets, no inline styles
**Plans:** 4/5 plans executed

Plans:
- [x] 02-01-PLAN.md — Foundation: activate nav placeholders in index.html as real anchors; add content-entry CSS classes (.pub-entry, .talk-entry, .writing-year-group, .writing-entry, .code-intro) to style.css
- [ ] 02-02-PLAN.md — publications.html: extract 7 publications verbatim from wayback Publications.html (title, authors, venue, Paper/Code links, TLDR)
- [x] 02-03-PLAN.md — talks.html: extract 3 talks verbatim from wayback Talks.html (title, venue, year, Video/Slides/Feature links)
- [x] 02-04-PLAN.md — writing.html: extract writing entries verbatim from wayback Writing.html grouped by year; drop broken WordPress-placeholder anchors per CONT-09
- [x] 02-05-PLAN.md — code.html: short page pointing to github.com/ivyclare per UI-SPEC §Copywriting Contract (no auto-redirect, explicit link only)

**UI hint:** yes

### Phase 3: Polish & Verification
**Goal:** The site is verifiably ship-ready — readable on a 375px mobile viewport, accessible (WCAG AA contrast, semantic HTML), free of broken links and Wayback prefixes, free of trackers, and accompanied by a README explaining how to edit and rebuild.
**Mode:** mvp
**Depends on:** Phase 2
**Requirements:** CONT-08, CONT-09, DES-04, DES-08, DES-09, QUAL-01, QUAL-02, QUAL-03, QUAL-04, QUAL-05, QUAL-06, DOC-01
**Success Criteria** (what must be TRUE):
  1. Every page is readable and usable at a 375px-wide viewport (single column reflow, no horizontal scroll, tap targets reachable) and uses semantic HTML (`<nav>`, `<main>`, `<section>`, proper heading hierarchy)
  2. The chosen teal `--accent` value is documented as passing WCAG AA contrast against the page background, verified with a named contrast check
  3. Every internal link between the 5 pages resolves, every external link resolves to its canonical destination with no `web.archive.org/...` prefix remaining, and any sections that were incomplete in the Wayback references are flagged for the owner rather than fabricated
  4. Opening any of the 5 pages as a local file produces no JavaScript console errors, no analytics/tracking scripts are present, and images are sized small enough for fast first paint
  5. `README.md` at the repo root explains how the site is built, where content lives, and how to preview locally
**Plans:** TBD
**UI hint:** yes

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Skeleton + Home MVP | 4/4 | Complete | 2026-05-19 |
| 2. Remaining Content Pages | 5/5 | Complete | 2026-05-19 |
| 3. Polish & Verification | 0/TBD | Not started | - |
