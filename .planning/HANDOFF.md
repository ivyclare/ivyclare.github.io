# HANDOFF — ivolinengong.com rebuild

**Last updated:** 2026-05-19, mid-Phase-1 setup (after UI-phase invocation, before UI-SPEC.md exists)
**Author of handoff:** Claude (Opus 4.7, GSD orchestration session)
**Read this first** if you're resuming this project in a fresh session.

---

## TL;DR — Where We Are

Project initialization (`/gsd-new-project`) is **done**. Roadmap exists. We are **about to run `/gsd-ui-phase 1`** to produce the UI design contract for Phase 1, then will go through `/gsd-plan-phase 1` → `/gsd-execute-phase 1` to actually build the Home page MVP.

**Nothing is built yet.** No `index.html`, no `style.css`, no `CNAME`. Only planning artifacts and source materials.

---

## What This Project Is

Rebuild the lost `ivolinengong.com` (academic personal site, owner = Ivoline Ngong, PhD candidate at UVM + research scientist at OpenMined) as a fully static GitHub Pages site served from `ivyclare.github.io` at the custom domain `ivolinengong.com`. Content is extracted **verbatim** from Wayback Machine HTML snapshots — no fabrication. The original ran on WordPress and was killed by hosting suspension; static-only is the architectural mitigation. See `PROJECT-BRIEF.md` (repo root) for the canonical owner-written brief.

## Key Decisions Already Locked In (mid-session)

| # | Decision | Source |
|---|----------|--------|
| 1 | **5 pages** (Home, Publications, Talks, Writing, Code), not single-page | Owner answered in questioning |
| 2 | **Plain HTML** committed directly, no Jekyll, no SSG | Owner answered in questioning |
| 3 | **Repo `ivyclare.github.io`** (user-site repo, served at root) | Owner answered in questioning |
| 4 | **Include small profile photo** near header | Owner answered in questioning |
| 5 | **Code page redirects/links to GitHub profile** (`github.com/ivyclare`) | Owner answered in questioning |
| 6 | **Teal accent** as a single CSS custom property `--accent`, WCAG AA against white | Locked in brief §5 |
| 7 | **News section is scrollable** (fixed-height container, internal scroll) — NOT a long inline dump of all entries | Owner clarified post-roadmap |
| 8 | **CV is a real PDF** (`Ivoline_Ngong_CV_August_2025.pdf`) — CV link in Home page links to `assets/Ivoline_Ngong_CV_August_2025.pdf` | Owner provided file post-roadmap |
| 9 | **Vertical MVP** phase structure — each phase ships an end-to-end working slice | Owner picked in Step 7.5 |
| 10 | **YOLO config mode, coarse granularity, parallel execution, no per-phase research** | Configured in `.planning/config.json` |

## Files in the Repo Right Now

**Owner-provided inputs** (already present when session started):
- `PROJECT-BRIEF.md` — canonical written brief, treat as source of truth
- `profile_pic.png` — owner's headshot (will move to `assets/profile_pic.png`)
- `Ivoline_Ngong_CV_August_2025.pdf` — CV PDF (will move to `assets/Ivoline_Ngong_CV_August_2025.pdf`)
- `reference/wayback/` — 4 saved HTML pages from Internet Archive (Home, Publications, Talks, Writing) + asset folders. **Authoritative content source.**
- `reference/screenshot/` — 11 PNG screenshots for visual reference

**Created during this session** (committed to `main`):
- `.planning/PROJECT.md` — project context, requirements buckets, decisions
- `.planning/REQUIREMENTS.md` — 39 v1 requirements with REQ-IDs (CONT-01..09, PAGE-01..08, DES-01..09, INFRA-01..06, QUAL-01..06, DOC-01) + traceability table mapping each to a phase
- `.planning/ROADMAP.md` — 3 phases, vertical-MVP mode
- `.planning/STATE.md` — project memory
- `.planning/config.json` — GSD workflow config
- `CLAUDE.md` — auto-generated project guide
- `.git/` — git repo initialized, branch `main`

**Not yet created:** any site files (`index.html`, `style.css`, `CNAME`, `assets/`, `README.md`).

## Roadmap At a Glance

| Phase | Goal | # Reqs | Status |
|-------|------|--------|--------|
| **1. Skeleton + Home MVP** | Working `index.html` (Home page with scrollable news + CV link) + repo skeleton + shared teal CSS, deployable as-is | 19 | Not started |
| **2. Remaining Content Pages** | Publications, Talks, Writing, Code pages + shared nav | 8 | Not started |
| **3. Polish & Verification** | 375px responsive, WCAG AA, link sweep, semantic HTML, README | 12 | Not started |

Full details: `.planning/ROADMAP.md`. All requirements 100% mapped to a phase.

## Exact Next Step If Resuming Cold

1. **Verify nothing was built yet:** `ls /Users/ivolinengong/Documents/My_Website` — should see `PROJECT-BRIEF.md`, `profile_pic.png`, `Ivoline_Ngong_CV_August_2025.pdf`, `reference/`, `.planning/`, `CLAUDE.md`, `.git/`. If you also see `index.html`, someone already started Phase 1 execution — re-read `.planning/STATE.md` to find where they stopped.
2. **Re-read these in order:**
   - `.planning/PROJECT.md` (5 min) — what we're building and why
   - `.planning/REQUIREMENTS.md` (5 min) — the 39 testable requirements
   - `.planning/ROADMAP.md` (3 min) — Phase 1 success criteria are 6 numbered items
   - `PROJECT-BRIEF.md` (5 min) — owner's original brief, hard constraints
3. **Run `/gsd-ui-phase 1`** to produce `.planning/phases/01-skeleton-home-mvp/UI-SPEC.md` — the design contract specifying typography scale, color tokens, spacing rhythm, breakpoints, the scrollable news container behavior, etc.
4. **Then `/gsd-plan-phase 1`** — produces `PLAN.md` with task breakdown.
5. **Then `/gsd-execute-phase 1`** — actually builds Phase 1.

## Critical Reminders for Execution (Don't Re-Litigate)

- **Content fidelity is non-negotiable.** Extract verbatim from `reference/wayback/*.html`. Do NOT paraphrase, condense, or invent. If a section is incomplete in references, flag it and ask the owner.
- **Strip Wayback prefixes** from every recovered link before writing it to HTML. Anything starting with `https://web.archive.org/web/...` needs the prefix removed so the canonical URL is preserved.
- **No frameworks, no CDNs, no build step.** Plain HTML + CSS + at most tiny vanilla JS. Self-host any fonts. The repo must work on GitHub Pages with zero config.
- **`CNAME` file at repo root must contain exactly `ivolinengong.com`** (one line, no trailing comment).
- **Move assets into `assets/`** before linking — don't link to root-level `profile_pic.png` or `Ivoline_Ngong_CV_August_2025.pdf`.
- **News section: scrollable container**, not a 4-year inline dump.
- **CV link target: `assets/Ivoline_Ngong_CV_August_2025.pdf`**.
- **Teal accent: define once, use everywhere.** Verify WCAG AA contrast against white. Apply to links + section headings + accents.
- **DNS, repo push, Pages enablement are OUT OF SCOPE** — owner does these manually post-build (see `PROJECT-BRIEF.md` §10).
- **Out-of-scope list is explicit** in `REQUIREMENTS.md` and `PROJECT.md` — don't drift back into them.

## Git History (commits already on `main`)

```
d1b7f93 docs: create roadmap (3 phases)
9bcb4bd docs: define v1 requirements
17f05aa chore: add project config
65a82f3 docs: initialize project
```

A 5th commit will land after this handoff (artifact updates for scrollable news + CV PDF + this HANDOFF.md).

## GSD Workflow Config (from `.planning/config.json`)

- mode: YOLO (auto-approve, just execute)
- granularity: coarse (3–5 broad phases)
- parallelization: true
- commit_docs: true (planning lives in git alongside code)
- model_profile: balanced (Sonnet for most agents)
- workflow.research: false (no per-phase research — domain is straightforward)
- workflow.plan_check: true
- workflow.verifier: true
- workflow.nyquist_validation: false (not used at coarse granularity)
- ship.pr_body_sections: [] (no PR template — personal site)

## Open Threads (things deliberately left for execution)

- **Exact teal hex value** — not picked yet. UI-spec phase should pick one and verify WCAG AA.
- **System font stack vs single self-hosted font** — UI-spec phase decides.
- **Scrollable-news max-height** — UI-spec phase decides (e.g., `60vh` or fixed px).
- **Code page format** — instant `<meta http-equiv="refresh">` redirect, or a short page with an explicit link? Recommend the latter (graceful degrade), but either is acceptable.
- **CV link `target="_blank"` vs same tab** — UI-spec phase decides.

## If You Need to Diverge from the Plan

If anything in `.planning/REQUIREMENTS.md` or `.planning/ROADMAP.md` no longer makes sense, update those files **before** writing code. They are the contract. Add to the Key Decisions table in `.planning/PROJECT.md` so the rationale survives.

---

*End of handoff. Welcome back — the project is ready for Phase 1 design + build.*
