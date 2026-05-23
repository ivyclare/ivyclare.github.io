<!-- GSD:project-start source:PROJECT.md -->
## Project

**ivolinengong.com — Academic Personal Website Rebuild**

A faithful, fully-static rebuild of `ivolinengong.com` — the personal academic
site of Ivoline Ngong (PhD candidate, University of Vermont; research scientist
at OpenMined). The original ran on WordPress on suspended shared hosting with
no backup; this rebuild lives on GitHub Pages from the `ivyclare.github.io`
repo and serves at the custom domain `ivolinengong.com`. Content is recovered
verbatim from Wayback Machine HTML snapshots stored in `reference/`.

**Core Value:** **Faithful, durable recreation of the original site's content on infrastructure
that cannot be taken down.** If the site reads true to the original and stays
online for free with zero maintenance, this project succeeded.

### Constraints

- **Tech stack**: Plain HTML + CSS + at most a tiny amount of vanilla JavaScript — no frameworks, no build step, no server-side code. Reason: dynamic resource usage killed the previous site; static is the architectural mitigation.
- **Hosting**: Must work on GitHub Pages with zero configuration. The repo must serve as-is — no Jekyll config, no Actions workflow required to render. Reason: simplest possible operational footprint.
- **Domain**: Site must serve at `ivolinengong.com`. CNAME file at repo root contains exactly that string.
- **Dependencies**: No CDN-loaded frameworks required for page render. Self-host fonts (or use system font stack). Reason: independence from external services that can break or disappear.
- **Repo**: User-site repo `ivyclare.github.io` on GitHub user `ivyclare`. Served at the domain root.
- **Performance & accessibility**: Semantic HTML, mobile-responsive, fast first paint, WCAG AA contrast on the teal accent. No heavy images.
- **Content fidelity**: Extract all biographical/news/publications/talks/writing content verbatim from `reference/wayback/` HTML. Do NOT invent or paraphrase content. If a section is incomplete in references, flag it rather than fabricating.
<!-- GSD:project-end -->

<!-- GSD:stack-start source:STACK.md -->
## Technology Stack

Technology stack not yet documented. Will populate after codebase mapping or first phase.
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

Conventions not yet established. Will populate as patterns emerge during development.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

Architecture not yet mapped. Follow existing patterns found in the codebase.
<!-- GSD:architecture-end -->

<!-- GSD:skills-start source:skills/ -->
## Project Skills

No project skills found. Add skills to any of: `.claude/skills/`, `.agents/skills/`, `.cursor/skills/`, `.github/skills/`, or `.codex/skills/` with a `SKILL.md` index file.
<!-- GSD:skills-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->
