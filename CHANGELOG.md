# Changelog

All notable changes to this repository are documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [1.6] — 2026-04-19

### Added
- **Assumptions block** — before generating the bootstrap script, the AI must output a structured summary of everything it understood from the discussion and wait for explicit user confirmation. If the user corrects any field — the AI updates the block and asks again before proceeding.

### Why
The AI silently making assumptions was the main source of wrong bootstraps — wrong type, invented commands, misunderstood component split. Making assumptions explicit and requiring confirmation before any generation closes this gap. The block also surfaces which fields used safe defaults, so the user can catch them without reading the full output.

---

## [1.5] — 2026-04-19

### Changed
- **Project type system simplified: Minimal / Standard / Extended → Single / Multi-Component**
  - Single: one runtime, one process
  - Multi-Component: multiple runtimes or independent components
  - Decision rule: "Can this project run as a single process?" Yes → Single, No → Multi-Component
  - All references updated throughout the template (structure section names, checklists, safe defaults, bash requirements, when-to-extend table)

### Why
Three levels (Minimal/Standard/Extended) created a grey zone — Standard was ambiguous and teams debated which level to pick. The real distinction is architectural: one runtime vs. multiple runtimes. Two levels with a single yes/no question removes the ambiguity entirely. The "if unsure — pick simpler" rule is preserved.

---

## [1.4] — 2026-04-19

### Added
- Anti-patterns section covering common drift modes in daily use
- Three documented access patterns (external / submodule / snapshot) in "How to store and version this template"
- Explicit criteria for choosing Minimal / Standard / Extended project type
- Safe defaults table for parameters not discussed during bootstrap
- Size budget for CLAUDE.md (target 40–80 lines, ceiling 120)
- Reminder in commit.md to record architectural decisions in docs/decisions.md

### Changed
- Tightened guidance on `.claude/settings.json`: no placeholder `Bash(...)` entries
- Clarified status of empty `docs/standards/` folder vs. empty files inside it

### Fixed
- Outdated `BOOTSTRAP_VERSION="1.2"` in the bash-script skeleton

---

## [1.3] — 2026-04-19

### Added
- **New section in `project-bootstrap.md`: "When full bootstrap is NOT needed"** — guidance on selective application of the template
  - Explicit distinction: projects (where full bootstrap applies) vs meta-tools / notes / spec repos (where it doesn't)
  - The `claude-templates` repo itself is named as a real example of selective application
- `CLAUDE.md` added to the `claude-templates` repo itself — provides AI context when working on the template
- `.claude/commands/commit.md` added to the repo — adapted for template-specific commit flow (EN/RU sync check, version bumps, tag reminders)

### Why
Realized that applying the template to its own spec repo would be conceptually wrong — the repo is a meta-tool, not a project. It exists once, not repeatedly bootstrapped. But the insight is valuable beyond this repo: **the template is for projects, not for everything with code in it**. Added explicit guidance so future users (including future me) don't overapply. Also took only the parts that genuinely help — CLAUDE.md and commit command — as a demonstration of selective application.

### Not added (intentionally)
- `.claudeignore` — nothing to ignore in a markdown-only repo
- `docs/decisions.md` — `CHANGELOG.md` already plays that role here
- `docs/standards/` — empty folder with no real standards to record
- `bootstrap.sh` — a one-time setup repo needs no reproducible setup script

This "not added" list is intentional documentation — it shows what selective application looks like in practice.

---

## [1.2] — 2026-04-19

### Added
- **Language rule** as explicit principle #7: Discussion in Russian, code/docs in English
  - Dedicated section with a table of what goes in which language
  - Applied throughout all file templates (CLAUDE.md, README.md, commands, decisions.md)
  - Stop-list item: do not translate code or technical comments to Russian
- **Project type categorization** at bootstrap: Minimal / Standard / Extended
  - Each type has its own recommended structure
  - Base structure for Minimal/Standard, Extended structure for multi-component projects
  - AI must confirm the type before generation
- First `decisions.md` entry now explicitly mentions the language rule

### Why
Reviewed an alternative template that proposed the language rule as a core practice. This turned out to be the most valuable idea — it formalizes a pattern that was implicit before. Also adopted explicit project type selection to prevent accidental overengineering on simple projects while still supporting embedded/multi-component cases.

---

## [1.1] — 2026-04-19

### Added
- `docs/standards/` for living development standards, filled organically as real rules emerge
- `docs/meta/` (optional) for meta information like bootstrap snapshots in airgapped environments
- `docs/meta/` added to `.claudeignore` in all stack variants
- Guidance on how to version and store the template in a separate repository
- First entry in `docs/decisions.md` becomes the way to record template usage (no copying the template into the project)

### Changed
- Explicit principle: the template lives outside projects; usage traces live inside via `docs/decisions.md`
- Stop-list: do not copy this file into the project; do not create placeholder files in `docs/standards/`

### Why
Needed a clean way to reference which template version a project was bootstrapped with, without duplicating the template into every project. Landed on the external-template + decisions.md-link approach. Also added `docs/standards/` as a dedicated home for living standards, separating them from one-shot architecture decisions.

---

## [1.0] — 2026-04-19

### Added
- Initial version of `project-bootstrap.md`
- Base project structure: `README.md`, `CLAUDE.md`, `.claudeignore`, `docs/decisions.md`, `.claude/settings.json`, `.claude/commands/{review,commit}.md`
- File templates for all structural files
- `.claudeignore` variants for Python, Node.js/TypeScript, Go
- Bash script requirements with heredoc escaping guidance
- Principles: CLAUDE.md as entry point, docs/ as source of truth, minimalism by default
- "When to extend" table

---

## Versioning policy

- **Major (x.0.0)** — breaking changes to the template structure or philosophy
- **Minor (1.x.0)** — new principles, new sections, new stack support
- **Patch (1.2.x)** — typo fixes, wording improvements, no structural changes

Tag each release in git: `git tag v1.3 && git push --tags`

Reference tagged versions from projects' `docs/decisions.md`:
```
https://github.com/<you>/claude-templates/blob/v1.3/project-bootstrap.md
```
