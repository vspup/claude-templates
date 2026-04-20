# Changelog

All notable changes to this repository are documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [1.9] — 2026-04-20

### Added
- `arduino-profile.md` — Layer 1 stack profile for Arduino projects
  - Critical naming rule: `.ino` must match folder name
  - Toolchain decision table: Arduino IDE vs PlatformIO
  - Thin sketch principle with code example
  - Responsibility layers table (app/protocol/transport/board/sensors)
  - `docs/` vs `reference/` distinction by origin (created vs received)
  - Hardware-oriented extension for measurement/device projects
  - Arduino Assumptions block extension (board, toolchain, transport, protocol)
  - Arduino-specific anti-patterns
- Arduino added to Stack profiles table in `project-bootstrap.md`

### Why
Arduino projects have a specific failure mode: business logic ends up in `.ino` because it's the most visible file. The thin sketch principle directly addresses this. The hardware-oriented extension (`app/protocol/transport/board/sensors`) maps to real embedded measurement projects with UART/CAN/I2C stacks.

---

## [1.8] — 2026-04-20

### Added
- `## 🧩 Stack profiles (Layer 1)` section in `project-bootstrap.md` — Layer 0 now explicitly references available Layer 1 profiles with a table
- `Stack profile (Layer 1)` field in the "Before generation" checklist
- `Stack profile (Layer 1)` field in the Assumptions block format

### Why
Layer 1 profiles existed but Layer 0 had no knowledge of them — the link was one-directional. Now the AI reading Layer 0 knows to check whether a stack profile applies and to surface it in the Assumptions block before generating anything.

---

## [1.7] — 2026-04-20

### Added
- `python-profile.md` — Layer 1 stack profile for Python projects
  - Layered architecture model (Layer 0–3)
  - Minimal Python project structure anchored to `src/` layout
  - Responsibility layers table (core/cli/io/diagnostics/config/utils) with "when to add" criteria
  - Diagnostics vs Tests distinction
  - Hardware-oriented `core/api + protocol` / `io/transport` split
  - Python Assumptions block extension
  - Python-specific anti-patterns (non-duplicating Layer 0)
- README updated: python-profile.md listed in Contents with Layer 0/1 labels

### Why
Language profiles extend the core bootstrap without duplicating it. Python is the first profile — establishes the pattern for future stack profiles (Go, TypeScript, etc.). The layered model (0→1→2→3) makes it explicit what each file is responsible for and prevents profile files from becoming second copies of the core spec.

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
