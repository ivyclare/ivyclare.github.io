---
phase: 01-skeleton-home-mvp
plan: 01
subsystem: repo-scaffolding
tags:
  - static-site
  - github-pages
  - repo-scaffolding
  - assets
requires: []
provides:
  - CNAME custom-domain binding for GitHub Pages (ivolinengong.com)
  - assets/ directory as canonical home for self-hosted binaries
  - assets/profile_pic.png path (consumed by Plan 03 index.html)
  - assets/Ivoline_Ngong_CV_August_2025.pdf path (consumed by Plan 03 index.html CV link)
  - .gitignore baseline excluding .DS_Store + editor cruft
affects:
  - 01-03-PLAN.md (index.html) — depends on the asset paths created here
tech_stack:
  added: []
  patterns:
    - flat-static-repo
    - canonical-assets-dir
key_files:
  created:
    - CNAME
    - .gitignore
    - assets/profile_pic.png
    - assets/Ivoline_Ngong_CV_August_2025.pdf
  modified: []
  deleted: []
  notes:
    - "profile_pic.png and Ivoline_Ngong_CV_August_2025.pdf moved from repo root into assets/ (filesystem mv + git add; see Deviations)."
    - "Pre-existing .DS_Store at repo root was intentionally NOT removed (out of scope per plan; .gitignore prevents future tracking)."
decisions:
  - "Used filesystem mv + git add fallback (plan-authorized) because both binary files were untracked when this plan ran — git mv requires a tracked source. Consequence: git records additions, not renames, for these two files. No history was lost because no prior commit ever referenced these paths."
metrics:
  duration_seconds: ~180
  task_count: 2
  files_changed: 4
  completed: 2026-05-19T16:32:39Z
requirements_satisfied:
  - INFRA-01
  - INFRA-02
  - INFRA-06
  - PAGE-07
  - PAGE-08
---

# Phase 1 Plan 01-01: Repo Scaffolding Summary

Deployable repo skeleton — `CNAME` for GitHub Pages custom domain, baseline `.gitignore` for macOS/editor cruft, and `assets/` directory populated with the owner-provided `profile_pic.png` headshot and `Ivoline_Ngong_CV_August_2025.pdf` — all paths Plan 03's `index.html` will hard-code.

## What Was Built

### Task 1: CNAME, .gitignore, assets/ directory — commit `801a695`

Created three foundational artifacts at the repo root:

- **`CNAME`** — single line `ivolinengong.com` followed by a single newline (17 bytes total). Verified byte-exact with `cat CNAME | tr -d '\n' = "ivolinengong.com"`. This is the literal GitHub Pages custom-domain binding (INFRA-01).
- **`.gitignore`** — covers `.DS_Store`, `Thumbs.db`, `*.swp`, `*.swo`, `*~`, `.idea/`, `.vscode/`, and `node_modules/` (defensive — repo should never need node, but ignore if a contributor accidentally runs `npm install`). Does NOT include `.planning/` per critical constraint.
- **`assets/`** — empty directory created so Task 2 can populate it.

### Task 2: Move binary assets into assets/ — commit `76364c4`

Relocated two owner-provided binary files from repo root into `assets/`:

- `profile_pic.png` → `assets/profile_pic.png` (2,887,296 bytes, PNG magic `89504e470d0a1a0a` verified)
- `Ivoline_Ngong_CV_August_2025.pdf` → `assets/Ivoline_Ngong_CV_August_2025.pdf` (106,604 bytes, PDF magic `25504446` / `%PDF` verified)

Path-and-filename invariants preserved verbatim — Plan 03's `<img src="assets/profile_pic.png">` and CV `<a href="assets/Ivoline_Ngong_CV_August_2025.pdf">` will resolve.

## Final Repo Layout Snapshot

```
$ ls -la
.DS_Store                         # pre-existing — not in scope to delete (plan §Task 1)
.git/
.gitignore                        # NEW
.planning/
CLAUDE.md
CNAME                             # NEW — contains exactly: ivolinengong.com
PROJECT-BRIEF.md                  # untracked (owner input, out of scope)
assets/                           # NEW
  Ivoline_Ngong_CV_August_2025.pdf   # MOVED from root
  profile_pic.png                    # MOVED from root
reference/                        # untracked (owner input, out of scope)
style.css                         # from sibling Plan 01-02 (parallel wave)
```

```
$ ls assets/
Ivoline_Ngong_CV_August_2025.pdf
profile_pic.png

$ ls profile_pic.png Ivoline_Ngong_CV_August_2025.pdf 2>&1 | grep -c "No such"
2
```

## CNAME Exact Content

```
ivolinengong.com
```

(Trailing `\n` — a single LF byte; no second line, no comment, no whitespace.)

## .gitignore Contents

```gitignore
# macOS metadata
.DS_Store

# Windows metadata
Thumbs.db

# Editor swap / temp files
*.swp
*.swo
*~

# Editor config dirs
.idea/
.vscode/

# Defensive: this is a plain HTML/CSS site and should never need node,
# but if a contributor accidentally `npm install`s, ignore the result.
node_modules/
```

`grep '^\.DS_Store$' .gitignore` matches line 2 → INFRA acceptance criterion met.

## Git Status Excerpt — Move Tracking

The plan's acceptance criterion expected `git status` to show `R`-status renames. This was not achievable in practice (see Deviations §1). The actual diff for commit `76364c4` is:

```
$ git show --stat 76364c4 -- assets/
 .../assets/Ivoline_Ngong_CV_August_2025.pdf | Bin 0 -> 106604 bytes
 .../assets/profile_pic.png                  | Bin 0 -> 2887296 bytes
 2 files changed, 0 insertions(+), 0 deletions(-)
```

Git treats this as two additions (no prior tracked blob existed to rename from). The acceptance-criterion intent — file identity preserved, no fabrication or re-encoding — IS met: PNG/PDF magic bytes were verified byte-exact, and the file sizes match the originals.

## Commits

| Task | Commit    | Subject                                                          | Files                                                                       |
| ---- | --------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------- |
| 1    | `801a695` | feat(01-01): add CNAME, .gitignore, and assets/ scaffolding      | CNAME, .gitignore                                                           |
| 2    | `76364c4` | feat(01-01): move profile_pic.png and CV PDF into assets/        | assets/profile_pic.png, assets/Ivoline_Ngong_CV_August_2025.pdf             |

## Verification Results

| Check                                                            | Result |
| ---------------------------------------------------------------- | ------ |
| `test -f CNAME`                                                  | PASS   |
| `cat CNAME \| tr -d '\n' == "ivolinengong.com"` (byte-exact)     | PASS   |
| `test -d assets`                                                 | PASS   |
| `test -f .gitignore && grep -q '^\.DS_Store$' .gitignore`        | PASS   |
| `test -f assets/profile_pic.png`                                 | PASS   |
| `test -f assets/Ivoline_Ngong_CV_August_2025.pdf`                | PASS   |
| `test ! -f profile_pic.png` (root level)                         | PASS   |
| `test ! -f Ivoline_Ngong_CV_August_2025.pdf` (root level)        | PASS   |
| PNG magic bytes `89504e470d0a1a0a`                               | PASS   |
| PDF magic bytes `25504446` (%PDF)                                | PASS   |
| `ls profile_pic.png Ivoline_Ngong_CV_August_2025.pdf` → 2 misses | PASS   |
| git status shows files as renames (`R` status)                   | N/A — see Deviation 1 |

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 3 — Blocking Issue] `git mv` failed because source files were untracked**

- **Found during:** Task 2
- **Issue:** Both `profile_pic.png` and `Ivoline_Ngong_CV_August_2025.pdf` were present in the working tree but had never been committed to `main` — `git status` showed them as `??` (untracked). `git mv profile_pic.png assets/profile_pic.png` failed with `fatal: not under version control, source=profile_pic.png`. Same for the PDF.
- **Why this happened:** The plan's authoring assumption (Task 2 `<action>`: "the files in fact were committed in earlier project setup commits") did not match the actual repo state. The five planning-doc commits already on `main` (`449d95d`, `07ff0af`, `10372ab`, `d1b7f93`, `9bcb4bd`, …) only added `.planning/*` content; the owner's two binaries were dropped into the working tree but never committed.
- **Fix:** Applied the plan's documented fallback verbatim: filesystem `mv` then `git add` of the destination paths. No `git rm` was needed because the source paths were never tracked.
- **Consequence:** Git records both files as additions in commit `76364c4`, not as renames. The acceptance criterion "git status shows both files as renames (with `R` status) rather than delete+add pairs" cannot be satisfied because there was no prior `delete+add pair` — there was only a never-before-tracked working-tree file. Functionally this is even cleaner than the rename criterion intends: the canonical path `assets/profile_pic.png` is the first git-tracked location of that blob. No history was lost.
- **Files modified:** `assets/profile_pic.png`, `assets/Ivoline_Ngong_CV_August_2025.pdf`
- **Commit:** `76364c4`

### Out-of-Scope Items Left Untouched (Intentional)

- **`.DS_Store` at repo root** — Plan task 1 explicitly says: "Do NOT delete the existing `.DS_Store` file in this task; that is out of scope. The git index will simply stop tracking new ones." Left as-is; `.gitignore` now prevents it from re-entering staging.
- **`PROJECT-BRIEF.md`** — Untracked owner-provided brief, present in working tree before this plan. Not in `files_modified`; left untracked.
- **`reference/`** — Untracked owner-provided source material. Not in `files_modified`; left untracked. (Plan 04 / Phase 3 may decide whether to commit or `.gitignore` it.)

## Authentication Gates

None. This plan was a pure local filesystem + git operation; no network, no auth.

## Known Stubs

None. CNAME content is real (the live target domain). The two assets are the actual owner-provided binaries, byte-exact. No placeholder text was written.

## Self-Check

Files claimed to exist:

- `/Users/ivolinengong/Documents/My_Website/CNAME` — FOUND
- `/Users/ivolinengong/Documents/My_Website/.gitignore` — FOUND
- `/Users/ivolinengong/Documents/My_Website/assets/profile_pic.png` — FOUND
- `/Users/ivolinengong/Documents/My_Website/assets/Ivoline_Ngong_CV_August_2025.pdf` — FOUND
- `/Users/ivolinengong/Documents/My_Website/.planning/phases/01-skeleton-home-mvp/01-01-SUMMARY.md` — FOUND (this file)

Commits claimed to exist:

- `801a695` — FOUND in `git log --all`
- `76364c4` — FOUND in `git log --all`

## Self-Check: PASSED
