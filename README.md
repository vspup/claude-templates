# Claude Templates

Personal collection of templates and workflows for working with [Claude Code](https://docs.claude.com/en/docs/claude-code/overview).

**Repository:** https://github.com/vspup/claude-templates
**Translations:** [Русский](./README.ru.md)

---

## Contents

- [`project-bootstrap.md`](./project-bootstrap.md) — specification for bootstrapping a new project with Claude Code support (Layer 0)
  - [`project-bootstrap.ru.md`](./project-bootstrap.ru.md) — Russian translation
- [`python-profile.md`](./python-profile.md) — Python stack profile: structure, responsibility layers, packaging (Layer 1)

---

## How to use

1. Discuss the project idea with the AI, choose the stack, do initial analysis
2. At the end of the discussion, attach this file and ask:
   > "Generate a bash bootstrap script for the project based on this spec, taking our discussion into account."
3. Run the produced `bootstrap.sh`
4. `cd <project> && claude` — start working

---

## How to store and version

- **Keep the template outside your projects** — in a personal repository (e.g. `<you>/claude-templates` or `<you>/dotfiles`)
- **Version via git tags** — `v1.0`, `v1.1`, so you can reference an exact version
- **Do NOT copy this file into a project's `docs/`** — instead, record its use in `docs/decisions.md` with a link to the version
- **Evolve it** — add what works in practice, remove what doesn't

### Access patterns

Three ways to use the template from a project. Pick one:

1. **External reference (default).** Template lives in a separate repo. `docs/decisions.md` records the version used at bootstrap. No template files inside the project. Simplest, recommended for most cases.

2. **Git submodule.** Include the template as a submodule, e.g. at `.claude/templates/`. Useful if you want to pin an exact commit and update deliberately. Trade-off: adds submodule mechanics to everyday git operations.
   ```
   git submodule add https://github.com/vspup/claude-templates.git .claude/templates
   ```

3. **Snapshot in docs/meta/.** Copy the template file into `docs/meta/bootstrap-snapshot.md` at bootstrap time. Use this only in airgapped environments without access to the external repo. Add `docs/meta/` to `.claudeignore`.

Default to (1). Move to (2) or (3) only when there's a concrete reason.

---

## Language policy

This repository follows the same rule it promotes:

- **Canonical files:** English
- **Translations:** Russian, with `.ru.md` suffix

If an English and Russian version disagree — the English version wins.

Why: English gives portability and industry-standard readability. Russian translations exist for convenience when working in Russian-speaking context.

---

## Versioning

Semantic versioning via git tags: `v1.0`, `v1.1`, `v1.2`, ...

**When referencing a template from a project**, use a tagged URL:

```
https://github.com/vspup/claude-templates/blob/v1.4/project-bootstrap.md
```

This way your project's `docs/decisions.md` will always point to a stable snapshot, even if the template evolves later.

See [`CHANGELOG.md`](./CHANGELOG.md) for version history.

---

## Philosophy

> Claude is not a chatbot — it is an execution engine inside a structured system.

Templates here aim to define that structure with a minimal set of files. Start minimal, extend on demand.

---

## License

MIT
