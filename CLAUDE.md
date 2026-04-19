# Claude Templates — Repository Context

This repository is a meta-tool: a collection of templates and workflows for working with Claude Code. It is **not a typical project** — do not apply `project-bootstrap.md` to it fully. See "Scope" below.

## Language rule
- Discussion with AI: Russian
- Code, docs, commits, interfaces: English
- Canonical files: English. Russian translations have `.ru.md` suffix.
- On EN/RU divergence — EN wins.

## Contents
- `project-bootstrap.md` — canonical spec for bootstrapping new projects (EN)
- `project-bootstrap.ru.md` — Russian translation
- `README.md` / `README.ru.md` — repo entry points
- `CHANGELOG.md` — version history with "Why" sections (serves as this repo's decisions log)
- `LICENSE` — MIT

## Scope
This repo holds the spec, not projects that use the spec.
- The spec lives here.
- Usage traces live in each consuming project's `docs/decisions.md`.
- Do NOT copy templates from this repo into consumer projects.

## Versioning
Semantic versioning via git tags: `v1.0`, `v1.1`, `v1.2`.

- **Major (x.0.0)** — breaking changes to structure or philosophy
- **Minor (1.x.0)** — new principles, sections, stack support
- **Patch (1.2.x)** — wording, typos, no structural changes

Workflow for a new version:
1. Update `project-bootstrap.md` (canonical, EN) first
2. Sync `project-bootstrap.ru.md`
3. Update `CHANGELOG.md` with a "Why" note explaining motivation
4. Bump version number in both template files' headers
5. Commit, then tag: `git tag v<X.Y> && git push --tags`

## Translation sync
When the canonical EN file changes:
- Translate into `project-bootstrap.ru.md` preserving structure and version
- Keep the "translation of English" disclaimer in the Russian header
- Never let versions in EN/RU headers drift

## Commit style
Conventional Commits, in English:
- `feat:` new content or principles in a template
- `fix:` corrections to existing content
- `docs:` README / CHANGELOG updates that aren't about templates themselves
- `chore:` housekeeping

## Important
- This repo is the source of truth for the template. Do not edit template copies that may exist in other projects — update here, retag, reference the new tag.
- Before large edits — read `CHANGELOG.md` to understand the evolution and avoid re-litigating past decisions.
- Don't add `.claudeignore`, `docs/decisions.md`, `docs/standards/`, or `bootstrap.sh` here. They belong in consumer projects, not in the spec repo.
