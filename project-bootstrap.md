# Project Bootstrap for Claude Code

**Version:** 1.4
**Updated:** 2026-04-19
**Canonical:** this file
**Translations:** [Русский](./project-bootstrap.ru.md)
**Purpose:** Specification for bootstrapping a new project with Claude Code support.

## How to use

1. Discuss the project idea with the AI, choose the stack, do initial analysis
2. At the end of the discussion, attach this file and ask:
   > "Generate a bash bootstrap script for the project based on this spec, taking our discussion into account."
3. Run the produced `bootstrap.sh`
4. `cd <project> && claude` — start working

## How to store and version this template

- **Keep the template outside your projects** — in a personal repository (e.g. `<you>/claude-templates` or `<you>/dotfiles`)
- **Version via git tags** — `v1.0`, `v1.1`, so you can reference an exact version
- **Do NOT copy this file into a project's `docs/`** — instead, record its use in `docs/decisions.md` with a link to the version
- **Evolve it** — add what works in practice, remove what doesn't

### Access patterns

Three ways to use the template from a project. Pick one:

1. **External reference (default).** Template lives in a separate repo. `docs/decisions.md` records the version used at bootstrap. No template files inside the project. Simplest, recommended for most cases.

2. **Git submodule.** Include the template as a submodule, e.g. at `.claude/templates/`. Useful if you want to pin an exact commit and update deliberately. Trade-off: adds submodule mechanics to everyday git operations.
   ```
   git submodule add <template-repo-url> .claude/templates
   ```

3. **Snapshot in docs/meta/.** Copy the template file into `docs/meta/bootstrap-snapshot.md` at bootstrap time. Use this only in airgapped environments without access to the external repo. Add `docs/meta/` to `.claudeignore`.

Default to (1). Move to (2) or (3) only when there's a concrete reason.

---

## 🎯 Principles

1. **CLAUDE.md is the AI entry point** — concise (one screen max)
2. **docs/ is the source of truth** — details go here
3. **.claude/ holds rules and commands** — no duplication of docs/
4. **.claudeignore saves tokens** — the `.gitignore` analogue for AI
5. **Minimalism** — add only what was discussed
6. **Living standards → `docs/standards/`**, meta info → `docs/meta/`
7. **Language rule** — discussion in Russian, everything else in English

> Bootstrap defines the start of a project, not its evolution.
> Start minimal. Extend on demand.

---

## 🌐 Language rule

A single rule for every project bootstrapped from this template:

```
Discussion (AI chat, notes, conversations): Russian
Code, docs, comments, commits, interfaces, API, JSON: English
```

**In practice:**

| Artifact | Language |
|---|---|
| AI chat in Claude Code | Russian |
| Personal notes, chat TODOs | Russian |
| Source code | English |
| Code comments | English |
| Variable, function, file names | English |
| `README.md`, `CLAUDE.md`, `docs/*` | English |
| Commit messages (Conventional Commits) | English |
| API endpoints, JSON keys | English |
| End-user UI copy | as the product requires |

**Why:**
- Thinking speed — we think and discuss in the native language
- Industry standard — English code and docs are portable
- No encoding headaches in logs, commits, filenames
- The project can be handed off to any international colleague without rework

**Exception:** end-user content (UI copy, marketing) follows the product's target language, separate from technical documentation.

---

## 📋 Project types

Pick the type at start — it determines the initial structure.

### 🟢 Minimal
One technology, one runtime, simple task.
Examples: CLI utility, small script, single-page site, simple library.

**Structure:** base (see below).

### 🟡 Standard
Web service, API, ML project, desktop app — one primary stack, clear architecture.
Examples: FastAPI service, React app, CLI with plugins, ETL pipeline.

**Structure:** base + `docs/architecture.md` if needed.

### 🔴 Extended
Multiple components, different runtimes, interaction protocols.
Examples: firmware + software + web UI, microservices, embedded projects.

**Structure:** base + `docs/protocols/` + `reference/` + top-level component split (`firmware/`, `software/`, etc.).

### How to choose the type

Use these criteria in order. First match wins:

| Criterion | Type |
|---|---|
| 2+ top-level components or 2+ runtime domains (e.g. firmware + desktop app) | Extended |
| One primary deployable component with tests/lint/docs | Standard |
| One runtime, no deployable split, single artifact | Minimal |

If in doubt between two levels, pick the simpler one. The template is explicitly designed to extend on demand — starting smaller is cheaper than starting larger.

---

## 🚫 When full bootstrap is NOT needed

This template is designed for **projects** — things you build, deploy, or ship. Not everything that has a git history is a project in that sense. Applying the full bootstrap to non-projects creates empty folders, meaningless files, and conceptual confusion.

### Not projects — don't apply fully

- **Spec / template repositories** — like this one. They hold specifications, not things built from them. Bootstrap script is pointless (repo exists once). `decisions.md` duplicates `CHANGELOG.md`. `docs/standards/` has no standards to hold.
- **Personal wikis and notes** — Obsidian vaults, Zettelkasten, knowledge bases. Different structure needs.
- **Dotfiles** — your shell config is not a project.
- **Documentation-only repos** — unless they're documenting a real software product as part of it.
- **One-off scripts** — a single `.py` file solving a one-time problem doesn't need `.claude/commands/` and `docs/decisions.md`.

### Selective application

For meta-tools and non-project repos you can still take **parts** of the template when they help:
- **`CLAUDE.md`** — almost always useful; gives AI context for working with the repo
- **`.claude/commands/`** — useful if you have repeating chat workflows (commit formatting, review conventions)
- **Language rule** — useful anywhere

And skip what doesn't fit. This is **not a failure mode** — it's the correct application of the principle "minimalism, extend on demand."

### Real example

The `claude-templates` repository itself (where this file lives) applies the template **selectively**: it has `CLAUDE.md` and `.claude/commands/commit.md`, but intentionally has no `.claudeignore`, `docs/decisions.md`, `docs/standards/`, or `bootstrap.sh`. Instead, `CHANGELOG.md` plays the role of a decisions log. See this repo as a reference for what selective application looks like.

---

## 📁 Base structure (Minimal / Standard)

```
<project>/
├── README.md
├── CLAUDE.md
├── .claudeignore
├── .gitignore
├── docs/
│   ├── decisions.md
│   └── standards/          ← living standards, filled organically
├── .claude/
│   ├── settings.json
│   └── commands/
│       ├── review.md
│       └── commit.md
└── <source directory for the stack>
```

## 📁 Extended structure (only when explicitly needed)

```
<project>/
├── README.md
├── CLAUDE.md
├── .claudeignore
├── .gitignore
├── docs/
│   ├── decisions.md
│   ├── standards/
│   └── protocols/          ← if components interact
├── reference/              ← external materials (datasheets, vendor docs)
├── .claude/
│   ├── settings.json
│   └── commands/
│       ├── review.md
│       └── commit.md
├── firmware/               ← if embedded
├── software/               ← if there is a distinct software layer
└── tools/                  ← utilities, converters, automation
```

**Note:** `docs/standards/` starts empty. It is filled as real rules emerge. In the Extended structure we also don't create stub directories — only real ones.

---

## ✅ Before generation, the AI must confirm

- Project name (folder name)
- **Project type:** Minimal / Standard / Extended
- Short description (1–2 sentences)
- Main language and version
- Key libraries / framework
- Commands: run / tests / lint
- Bootstrap template version (for the `decisions.md` entry)
- For Extended: which components (firmware/software/tools), whether protocols/reference are needed
- Any specifics not covered by the template

If a **critical** parameter wasn't discussed — ask. Critical parameters: project name, type (Minimal/Standard/Extended), primary language, main runtime components.

For non-critical parameters — apply safe defaults (see below) and explicitly list them in the final bootstrap output so the user can correct them.

### Safe defaults

When a parameter wasn't discussed, use these defaults instead of inventing values:

| Parameter | Safe default |
|---|---|
| Lint command | Omit from `CLAUDE.md`. Do not invent. |
| Test command | Write `Tests: not configured yet` in `CLAUDE.md`. |
| Run command | If unknown, write `Run: see README.md` and leave README minimal. |
| Project type (when ambiguous) | Pick the simpler option (Minimal over Standard, Standard over Extended). |
| `.claude/settings.json` `allow` list | Include only safe git-read operations (`git status`, `git diff`, `git log`). Do not add placeholder bash patterns for tools whose commands were not discussed. |
| Naming of top-level folders | Lowercase kebab-case. Avoid generic names like `misc`, `stuff`, `temp`, `utils` at top level. |
| Framework-specific configs (Docker, CI, pre-commit) | Do not add unless explicitly requested. |

At the end of bootstrap, print a "Defaults applied" summary listing every field where a default was used instead of a discussed value.

---

## ❌ What NOT to do during generation

- Do not copy this file into the project
- Do not create empty "future" directories (except `docs/standards/` as a placeholder for future standards)
- Do not create stubs like `TECH_SPEC.md` or `ARCHITECTURE.md` if not discussed
- Do not add `firmware/`, `software/`, `tools/` without explicit Extended type confirmation
- Do not add Dockerfile / docker-compose without request
- Do not add CI/CD configs without request
- Do not add pre-commit hooks without request
- Do not install dependencies that weren't discussed
- Do not invent conventions that weren't in the discussion
- Do not add extra commands to `.claude/commands/`
- Do not create placeholder files in `docs/standards/`
- Do not translate code or technical comments into Russian
- Do not generate placeholder `Bash(...)` allow-entries in `.claude/settings.json` for commands that weren't discussed
- Do not invent lint/test/run commands — omit them or mark as not configured

---

## 📄 File templates

### CLAUDE.md

```markdown
# <Project Name>

<1-2 sentence description>

## Language rule
- Discussion with AI: Russian
- Code, docs, comments, commits, interfaces: English

## Stack
- <language/version>
- <key libraries>

## Commands
- Run: `<command>`
- Tests: `<command>`
- Lint: `<command>`

## Structure
- `<dir>/` — <purpose>
- `docs/decisions.md` — architecture decisions
- `docs/standards/` — living development standards (if any)

## Conventions
- <main code rules from discussion>

## Important
- Before large changes — read `docs/decisions.md`
- <what not to touch, if discussed>
```

**Note:** CLAUDE.md itself is written in English, but the language-rule section makes the policy explicit for the AI.

**Size budget:** target 40–80 lines. Hard ceiling 120 lines. If content grows past the ceiling — move detail into `docs/` (architecture, standards) and keep CLAUDE.md as an index with links.

---

### README.md

```markdown
# <Project Name>

<Description for humans>

## Installation

<installation steps for the stack>

## Usage

<minimal example>

## Development

- `CLAUDE.md` — AI assistant context
- `docs/decisions.md` — architecture decisions
- `docs/standards/` — development standards

## Language policy

Discussion in chat/issues: Russian.
Code, documentation, commits, interfaces: English.
```

---

### .claudeignore — Python

```
# Environments
venv/
.venv/
env/

# Caches
__pycache__/
*.pyc
.pytest_cache/
.mypy_cache/
.ruff_cache/

# Build
dist/
build/
*.egg-info/

# Secrets
.env
.env.*
secrets/

# Data and logs
data/
logs/
*.log
*.csv
*.sqlite

# IDE
.vscode/
.idea/

# Lock files
poetry.lock
uv.lock

# Meta info (snapshots, not used in work)
docs/meta/
```

---

### .claudeignore — Node.js / TypeScript

```
# Dependencies
node_modules/

# Build
dist/
build/
.next/
.nuxt/
out/

# Caches
.turbo/
.cache/
coverage/

# Secrets
.env
.env.*

# Logs
*.log

# IDE
.vscode/
.idea/

# Lock files
package-lock.json
pnpm-lock.yaml
yarn.lock

# Meta info
docs/meta/
```

---

### .claudeignore — Go

```
# Binaries
bin/
*.exe
*.test
*.out

# Vendor
vendor/

# Caches
.cache/
coverage.out

# Secrets
.env
.env.*

# IDE
.vscode/
.idea/

# Meta info
docs/meta/
```

---

### .claude/settings.json

```json
{
  "permissions": {
    "allow": [
      "Bash(<test command>:*)",
      "Bash(<lint command>:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(git push:*)",
      "Bash(git reset --hard:*)"
    ]
  }
}
```

Adapt `allow` to the **actual commands the project uses**. If test/lint/run commands weren't discussed during bootstrap, leave only the safe git-read entries shown above. Do not add placeholder patterns like `Bash(<test command>:*)` — the AI will treat them as real commands later.

---

### .claude/commands/review.md

Written in Russian because this is an AI-chat instruction, not source code or documentation. It aligns with the language rule: discussion is in Russian.

```markdown
Проведи code review текущих изменений.

Шаги:
1. Запусти `git diff` и изучи изменения
2. Проверь соответствие конвенциям из CLAUDE.md
3. Если есть `docs/standards/` — свери с применимыми стандартами
4. Проверь правило языка: код, комментарии, docstrings — на английском
5. Запусти линт и тесты
6. Убедись, что нет утечек секретов
7. Дай резюме на русском: что хорошо, что исправить (указывай файл:строка)

Будь конкретным. Не переписывай код без запроса — только предлагай.
```

---

### .claude/commands/commit.md

```markdown
Создай коммит по текущим изменениям.

1. Запусти `git status` и `git diff --staged`
2. Сгенерируй сообщение на английском в стиле Conventional Commits:
   - `feat:` новая функциональность
   - `fix:` исправление бага
   - `refactor:` рефакторинг без изменения поведения
   - `test:` тесты
   - `docs:` документация
   - `chore:` рутина (зависимости, конфиги)
3. Если изменения содержат архитектурное решение (выбор библиотеки, смена подхода, новый протокол, отказ от подхода) — спроси: "записать это решение в docs/decisions.md?". Не блокируй коммит.
4. Покажи сообщение ПЕРЕД коммитом
5. После моего подтверждения — `git commit`
```

---

### docs/decisions.md

```markdown
# Architecture Decisions

Chronological log of important project decisions.
Each non-trivial decision = one entry.

---

## <YYYY-MM-DD>: Project bootstrap

**Context:** new project start
**Decision:** bootstrapped using project-bootstrap v<VERSION>
**Source:** <link to the template version tag in your repository>
**Consequences:**
- Structure: `.claude/`, `docs/decisions.md`, minimal CLAUDE.md
- Conventions fixed in CLAUDE.md
- Language rule: Discussion — Russian, Code/docs — English
- Living standards go to `docs/standards/` as they emerge

---

## <YYYY-MM-DD>: <next decision name>

**Context:** <why the question came up>
**Decision:** <what was chosen>
**Alternatives:** <what was considered and rejected>
**Consequences:** <what this means for code and development>
```

**Important:** the first bootstrap entry is required — it records template usage and the language rule without copying the template into the project.

---

### docs/standards/ — living standards

A folder for **development standards actually applied in work**.

**Put here:**
- `code-style.md` — code writing rules
- `git-workflow.md` — git workflow (branches, commits, PRs)
- `api-conventions.md` — API design
- `testing.md` — testing strategy
- More as real rules appear

**Do NOT put here:**
- Bootstrap templates or snapshots (those go to `docs/meta/` if needed)
- External documentation (datasheets, vendor docs — those go to `reference/`)
- Architecture decisions (those go to `docs/decisions.md`)

**Status of the folder itself:**
The folder is created empty at bootstrap as a reserved location — its existence signals "standards live here if any emerge". An empty folder is fine. Empty **files** are not.

**Filling principle:**
Don't create empty files. Create a file when:
1. A rule is already applied de-facto and needs to be recorded
2. You decide to standardize something new — write it down right at the moment of decision
3. Team > 1 person and synchronization is needed

---

### docs/meta/ — meta information (optional)

Created **only when needed**. Holds artifacts **about the project itself**, not about working with it:

- Bootstrap template snapshot, if you work in an airgapped environment without access to external repos
- One-off migration notes
- History of major restructurings

**Required:**
- Add `docs/meta/` to `.claudeignore` (so Claude doesn't spend tokens on it)
- Start each file with a note that this is meta information, not used in daily work

---

### .gitignore

Use a standard template for the chosen stack (GitHub gitignore templates).
Include everything from `.claudeignore` plus git-specific items.

**Do not add** `docs/meta/` to `.gitignore` — this info must be stored in the repo (but ignored by Claude).

---

## 🔧 Bash script requirements

The `bootstrap.sh` script must:

1. Start with `#!/usr/bin/env bash` and `set -euo pipefail`
2. Take the project name as the first argument: `bash bootstrap.sh my_project`
3. Refuse to overwrite an existing folder
4. Create directories for the chosen project type (Minimal/Standard/Extended)
5. Create all files via heredoc with correct escaping
6. **Fill the first entry in `docs/decisions.md`** with:
   - Today's date
   - Template version (from this file's header)
   - Link to the template version in your repository (if known)
   - Explicit mention of the language rule
7. Initialize git: `git init` + `git add .` + initial commit `chore: bootstrap project`
8. Finish by printing:
   - Path to the created project
   - Next steps (what to install, what to run)
   - Start command: `cd <project> && claude`

### Script skeleton

```bash
#!/usr/bin/env bash
set -euo pipefail

PROJECT_NAME="${1:-}"
if [[ -z "$PROJECT_NAME" ]]; then
  echo "Usage: bash bootstrap.sh <project_name>"
  exit 1
fi

if [[ -e "$PROJECT_NAME" ]]; then
  echo "Error: '$PROJECT_NAME' already exists"
  exit 1
fi

BOOTSTRAP_VERSION="1.4"
BOOTSTRAP_DATE="$(date +%Y-%m-%d)"

mkdir -p "$PROJECT_NAME"/{docs/standards,.claude/commands}
cd "$PROJECT_NAME"

# ... then create files via heredoc
```

### Heredoc notes

- Use `<<'EOF'` (single-quoted) for files containing `$`, `` ` ``, `\` so bash doesn't interpret them
- Use `<<EOF` (unquoted) only when you need variable substitution (e.g. `decisions.md` with date and version)

---

## 📈 When to extend the template

Add **only on demand** and only after discussion:

| Situation | What to add |
|---|---|
| Recurring complex tasks | `.claude/commands/feature.md` |
| A code rule has solidified | A file in `docs/standards/` |
| Lots of external documentation | `reference/` folder |
| Complex architecture | `docs/architecture.md` |
| Multiple modules with different rules | Nested `CLAUDE.md` |
| Team > 2 people | `docs/standards/git-workflow.md` |
| Specific protocols/interfaces | `docs/protocols/` |
| Standard → Extended transition | `firmware/` / `software/` / `tools/` |
| Airgapped environment | Snapshot in `docs/meta/bootstrap-snapshot.md` |

---

## 🚫 Anti-patterns in daily use

Bootstrap sets up the structure correctly. These are the ways projects drift from it over time. Catch them in review.

### Duplicating docs/ content into CLAUDE.md
CLAUDE.md is an entry point, not a mirror. If something belongs in `docs/architecture.md` or `docs/standards/`, link to it — don't inline it. The moment CLAUDE.md grows past its size budget, it has lost its job.

### Creating standards files "for the future"
`docs/standards/git-workflow.md` with a TODO inside is worse than no file. Empty standards files lie about what's actually enforced. Create the file the day the rule starts being applied — not earlier.

### Mixing languages inside a single artifact
Russian comments in English code. English labels on a Russian chat note. The language rule is per-artifact, not per-line. If you catch yourself switching mid-file, the file is in the wrong bucket.

### Storing large data without updating .claudeignore
Logs, datasets, vendor PDFs, build artifacts — any of these can silently inflate the AI's context budget. When adding a new directory that will grow, update `.claudeignore` in the same commit.

### Letting decisions.md fall behind
If a non-trivial architectural choice was made in chat and not recorded, it will be re-litigated in two weeks. Either record the decision or explicitly decide it wasn't worth recording — don't leave the log silently stale.

### Treating CLAUDE.md as permanent
Conventions change. Stack changes. CLAUDE.md that still describes the stack from month one after six months of evolution misleads the AI more than no CLAUDE.md at all. Update it when reality diverges.

### Adding commands to .claude/commands/ for one-off tasks
A command is a workflow you run repeatedly. A single "help me refactor this module" chat is not a command. Extract to a command only when the third repetition is coming.

### Committing AI-drafts into docs/
Intermediate AI output, chat exports, exploratory notes — none of this belongs in `docs/`. If it must be kept at all, put it in `docs/meta/` (which is in `.claudeignore`) or leave it out of the repo. `docs/` holds the final, authoritative state of the project.

### Nested CLAUDE.md without a real reason
Sub-directory `CLAUDE.md` files are for **actual differences in rules** — e.g. `firmware/` using C++ conventions while `software/` uses Python. Don't add nested CLAUDE.md for symmetry or because "every folder should have one". Top-level CLAUDE.md covers the whole project unless conventions genuinely diverge.

---

## 🧠 Philosophy

> Claude is not a chatbot — it is an execution engine inside a structured system.
> The template's job is to define that system with a minimal set of files.

**Storage and evolution rules:**
- One source of truth per artifact
- The template lives outside; usage traces live inside (`docs/decisions.md`)
- Standards are born from practice, not fantasy
- Think in Russian, write code in English
- Less is more

Extend as real needs emerge.

---

## 📋 Changelog

See [CHANGELOG.md](./CHANGELOG.md) in the repository root.
