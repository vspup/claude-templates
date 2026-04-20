# Python Profile (Layer 1)

**Extends:** [project-bootstrap.md](./project-bootstrap.md) (Layer 0)
**GitHub:** https://github.com/vspup/claude-templates/blob/main/python-profile.md

---

## Purpose

This profile extends the core bootstrap with Python-specific structure and conventions.
It does **not** repeat Layer 0 principles — it only adds what is Python-specific.

---

## How layers fit together

| Layer | File | Responsibility |
|---|---|---|
| 0 — Core | `project-bootstrap.md` | Language-agnostic foundation, rules, AI interaction model |
| 1 — Stack profile | `python-profile.md` ← this file | Python conventions, architectural model |
| 2 — Project shape | *(discussed per project)* | CLI / API / desktop / hardware tool |
| 3 — Optional modules | *(added on demand)* | Testing, linting, protocols, CI |

At bootstrap, attach Layer 0 + this file together.

---

## Minimal Python project

Start here. Nothing else until there is a real reason.

```
<project>/
├── src/
│   └── <package_name>/
│       └── __init__.py
├── pyproject.toml
├── README.md
├── CLAUDE.md
├── .claudeignore
├── .gitignore
├── docs/
│   └── decisions.md
└── .claude/
    ├── settings.json
    └── commands/
        ├── review.md
        └── commit.md
```

**Why `src/` layout:**
- avoids import shadowing during development
- aligns with PyPA best practices
- gives AI a clear code boundary

---

## Responsibility layers

Add a layer only when its responsibility becomes real in the project.

| Layer | Path | When to add |
|---|---|---|
| Business logic | `src/<pkg>/core/` | Almost always — add at start |
| CLI entry point | `src/<pkg>/cli/` | Project has a CLI interface |
| External interfaces | `src/<pkg>/io/` | Hardware, network, or filesystem I/O |
| Operational workflows | `src/<pkg>/diagnostics/` | Sequences that run against real systems |
| Configuration | `src/<pkg>/config/` | Non-trivial settings or modes |
| Shared helpers | `src/<pkg>/utils/` | Repeated utility logic appears |
| Tests | `tests/` | Testing is set up (outside `src/`) |

**Rule:** create a layer when its responsibility first appears — not before.

---

## Key distinctions

### Diagnostics vs Tests

| | Diagnostics | Tests |
|---|---|---|
| Part of product | Yes | No |
| Depends on real hardware | May | Must not |
| What it does | Executes workflows | Validates logic |
| Location | `src/<pkg>/diagnostics/` | `tests/` |

Diagnostics are features. Tests are development tools.

### core/ rule

No direct I/O in `core/`. All external interaction goes through `io/`.

```
cli/  →  core/  →  io/
              ↓
         diagnostics/
```

---

## Hardware-oriented extension

For device / embedded projects, split `io/` further:

```
src/<pkg>/
├── core/
│   ├── api/         ← high-level operations
│   └── protocol/    ← parsing and validation
└── io/
    └── transport/   ← UART / TCP / CAN
```

Add this split only when protocol and transport are genuinely separate concerns.

---

## Packaging

Baseline: `pyproject.toml` + `src/` layout.

Do not prescribe a specific tool (uv, poetry, pip) — that decision belongs to the project discussion and must appear in the Assumptions block.

---

## Python Assumptions extension

When bootstrapping a Python project, extend the standard Assumptions block with:

```
Assumptions:
- Project name: <name>
- Type: Single / Multi-Component
- Language: Python <version>
- Packaging tool: uv / poetry / pip (or "not discussed")
- Framework / key libraries: <list or "none">
- Run: <command or "see README.md">
- Tests: <command or "not configured yet">
- Lint: <command or "omitted">
- Layers present: <e.g. core, cli — only discussed ones>
- Defaults applied: <list or "none">

Proceed with bootstrap?
```

---

## Python-specific anti-patterns

Layer 0 covers general anti-patterns. These are Python-specific:

- Putting tests inside `src/`
- Mixing business logic with CLI argument parsing
- Treating diagnostics as tests or vice versa
- Using `utils/` as a dumping ground for unclassified code
- Creating the full layer tree before the project needs it
- Adding `io/` when there is no external I/O
