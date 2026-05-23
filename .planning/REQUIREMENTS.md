# Requirements: ivolinengong.com — Academic Personal Website Rebuild

**Defined:** 2026-05-19
**Core Value:** Faithful, durable recreation of the original site's content on infrastructure that cannot be taken down.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Content Extraction

- [x] **CONT-01**: Owner's name, role, and affiliation are extracted verbatim from `reference/wayback/Home - Ivoline Ngong.html` and rendered in the site header
- [x] **CONT-02**: Bio paragraph (PhD candidate at UVM, OpenMined research scientist, advised by Joe Near, research focus) is extracted verbatim from the Home reference HTML and rendered on the Home page
- [x] **CONT-03**: News/updates timeline (entries spanning ~2021–2025) is extracted verbatim from the Home reference HTML and rendered in reverse-chronological order inside a fixed-height **scrollable container** (the section scrolls internally rather than dumping all entries inline)
- [x] **CONT-04**: Contact links (email, Google Scholar, GitHub, LinkedIn, CV, etc.) are extracted verbatim from the Home reference HTML and rendered in a Links/contact section; the **CV link points to `assets/Ivoline_Ngong_CV_August_2025.pdf`** and opens the PDF (browsers may display inline or download depending on settings)
- [x] **CONT-05**: Publications list (papers, venues, links) is extracted verbatim from `reference/wayback/Publications - Ivoline Ngong.html` and rendered on the Publications page
- [x] **CONT-06**: Talks list is extracted verbatim from `reference/wayback/Talks - Ivoline Ngong.html` and rendered on the Talks page
- [x] **CONT-07**: Writing list is extracted verbatim from `reference/wayback/Writing - Ivoline Ngong.html` and rendered on the Writing page
- [ ] **CONT-08**: All Wayback `web.archive.org/...` prefixes are stripped from recovered links so links point at canonical destinations
- [ ] **CONT-09**: No biographical content is fabricated; sections incomplete in references are flagged for the owner rather than invented

### Pages

- [x] **PAGE-01**: `index.html` exists at repo root and serves as the Home page (header, bio, news, links)
- [x] **PAGE-02**: `publications.html` exists at repo root and serves the Publications page
- [x] **PAGE-03**: `talks.html` exists at repo root and serves the Talks page
- [x] **PAGE-04**: `writing.html` exists at repo root and serves the Writing page
- [x] **PAGE-05**: `code.html` exists at repo root, is a short page that points/redirects to the owner's GitHub profile, and is linked from the site navigation
- [x] **PAGE-06**: A shared navigation links all 5 pages from every page (consistent header or nav block)
- [x] **PAGE-07**: Small profile photo (`profile_pic.png` moved into `assets/`) appears near the header/bio on the Home page
- [x] **PAGE-08**: CV PDF (`Ivoline_Ngong_CV_August_2025.pdf` moved into `assets/`) is present at `assets/Ivoline_Ngong_CV_August_2025.pdf` and reachable via the Home page CV link

### Design & Styling

- [x] **DES-01**: A single shared `style.css` drives all pages — no inline styles, no per-page stylesheets
- [x] **DES-02**: An accessible teal hex value is chosen and defined once as a `--accent` CSS custom property
- [x] **DES-03**: Teal accent is applied consistently to links, section headings, and any dividers/header accents
- [ ] **DES-04**: Teal accent passes WCAG AA contrast against white/near-white background (verified)
- [x] **DES-05**: Body text uses neutral dark gray on white/near-white background
- [x] **DES-06**: Visual style follows Carlini-site spirit: single-column, content-first, minimal chrome, no decorative clutter
- [x] **DES-07**: Typography uses a system font stack OR a single self-hosted font — no externally-loaded webfonts that block rendering
- [ ] **DES-08**: Layout is responsive and readable on a ~375px-wide mobile viewport
- [ ] **DES-09**: Semantic HTML is used throughout (proper heading hierarchy, `<nav>`, `<main>`, `<section>`, etc.)

### Infrastructure & Hosting

- [x] **INFRA-01**: `CNAME` file exists at repo root containing exactly `ivolinengong.com`
- [x] **INFRA-02**: Repository structure matches PROJECT-BRIEF.md §7 (index.html, style.css, CNAME, assets/, README.md*, reference/) — *README.md deferred to Phase 3 DOC-01
- [x] **INFRA-03**: Repository serves correctly via GitHub Pages with zero configuration — no Jekyll config, no Actions workflow required to render
- [x] **INFRA-04**: No server-side code, no PHP, no database, no required build step
- [x] **INFRA-05**: No externally-loaded frameworks or CDN webfonts required for page render
- [x] **INFRA-06**: All site assets (images, fonts, CSS, optional JS) are self-hosted under `assets/`

### Quality & Verification

- [ ] **QUAL-01**: `index.html` opens correctly as a local file (`file://`) with no JavaScript console errors
- [ ] **QUAL-02**: All other pages also open correctly as local files with no console errors
- [ ] **QUAL-03**: Every internal link (between the 5 pages, anchors within a page) resolves with no 404s
- [ ] **QUAL-04**: Every external link resolves (no broken canonical URLs after Wayback prefix stripping)
- [ ] **QUAL-05**: No tracking scripts, analytics, ads, or third-party tracking are present in any page
- [ ] **QUAL-06**: Page weight is small (no heavy unoptimized images); first paint is fast

### Documentation

- [ ] **DOC-01**: `README.md` at repo root documents how the site is built/edited, where content lives, and how to preview locally

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### Enhancements

- **ENH-01**: Open Graph / social preview metadata for richer link unfurls
- **ENH-02**: RSS feed for the Writing page
- **ENH-03**: Print stylesheet for CV-style printing of the Home page
- **ENH-04**: Dark mode via `prefers-color-scheme`

## Out of Scope

Explicitly excluded. Documented to prevent scope creep.

| Feature | Reason |
|---------|--------|
| WordPress, CMS, admin dashboard | Original site killed by dynamic resource usage; static-only is the architectural mitigation |
| Server-side code, PHP, database, server build step | Same as above — must be inert static files |
| Jekyll or any static site generator | Brief prefers plain HTML committed directly; GitHub Pages must serve as-is with zero config |
| Externally-loaded webfonts or CDN frameworks blocking render | Independence from external services that can break or disappear |
| Analytics, ads, third-party tracking | Privacy and minimalism |
| Backend-powered contact form | `mailto:` link is sufficient |
| Visual redesign beyond "teal, Carlini-style" | Goal is faithful recreation, not reinvention |
| DNS configuration, hosting setup, HTTPS enforcement | Owner handles manually post-build (PROJECT-BRIEF.md §10) |
| Repo creation, push, GitHub Pages enablement | Owner handles manually post-build |

## Traceability

Every v1 requirement maps to exactly one roadmap phase. See ROADMAP.md for phase definitions and success criteria.

| Requirement | Phase | Status |
|-------------|-------|--------|
| CONT-01 | Phase 1 | Complete |
| CONT-02 | Phase 1 | Complete |
| CONT-03 | Phase 1 | Complete |
| CONT-04 | Phase 1 | Complete |
| CONT-05 | Phase 2 | Complete |
| CONT-06 | Phase 2 | Complete |
| CONT-07 | Phase 2 | Complete |
| CONT-08 | Phase 3 | Pending |
| CONT-09 | Phase 3 | Pending |
| PAGE-01 | Phase 1 | Complete |
| PAGE-02 | Phase 2 | Complete |
| PAGE-03 | Phase 2 | Complete |
| PAGE-04 | Phase 2 | Complete |
| PAGE-05 | Phase 2 | Complete |
| PAGE-06 | Phase 2 | Complete |
| PAGE-07 | Phase 1 | Complete |
| PAGE-08 | Phase 1 | Complete |
| DES-01 | Phase 1 | Complete |
| DES-02 | Phase 1 | Complete |
| DES-03 | Phase 1 | Complete |
| DES-04 | Phase 3 | Pending |
| DES-05 | Phase 1 | Complete |
| DES-06 | Phase 1 | Complete |
| DES-07 | Phase 1 | Complete |
| DES-08 | Phase 3 | Pending |
| DES-09 | Phase 3 | Pending |
| INFRA-01 | Phase 1 | Complete |
| INFRA-02 | Phase 1 | Complete |
| INFRA-03 | Phase 1 | Complete |
| INFRA-04 | Phase 1 | Complete |
| INFRA-05 | Phase 1 | Complete |
| INFRA-06 | Phase 1 | Complete |
| QUAL-01 | Phase 3 | Pending |
| QUAL-02 | Phase 3 | Pending |
| QUAL-03 | Phase 3 | Pending |
| QUAL-04 | Phase 3 | Pending |
| QUAL-05 | Phase 3 | Pending |
| QUAL-06 | Phase 3 | Pending |
| DOC-01 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 39 total
- Mapped to phases: 39 (100%)
- Unmapped: 0
- By phase: Phase 1 = 19, Phase 2 = 8, Phase 3 = 12

---
*Requirements defined: 2026-05-19*
*Last updated: 2026-05-19 after adding scrollable-news refinement + CV PDF asset (CONT-03 updated, CONT-04 updated, PAGE-08 added)*
