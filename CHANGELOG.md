# Changelog

All notable changes to this repository are documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [1.4] — 2026-04-19

### Added
- **Anti-patterns section** covering common drift modes in daily use: CLAUDE.md bloat, premature standards files, language mixing, missing .claudeignore updates, stale decisions.md, permanent CLAUDE.md, one-off commands
- **Three access patterns** for using the template from a project (external reference, git submodule, snapshot) — documented under "How to store and version this template"
- **decisions.md reminder in commit.md template** — step 3 asks to record architectural decisions without blocking the commit

### Why
Working with the template daily revealed patterns that degrade the structure over time but are invisible at bootstrap. Anti-patterns belong in the spec because they're exactly what code review should catch — and reviewers need a canonical list to point to. The access patterns section fills a gap: the template said "keep it outside your projects" but didn't explain what "outside" looks like in practice. The decisions.md reminder closes the loop between making a decision in chat and recording it — the most common way the decisions log silently falls behind.

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
