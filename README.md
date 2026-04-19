# Claude Templates

Personal collection of templates and workflows for working with [Claude Code](https://docs.claude.com/en/docs/claude-code/overview).

**Translations:** [Русский](./README.ru.md)

---

## Contents

- [`project-bootstrap.md`](./project-bootstrap.md) — specification for bootstrapping a new project with Claude Code support
  - [`project-bootstrap.ru.md`](./project-bootstrap.ru.md) — Russian translation

---

## How to use a template

1. Discuss the project idea with your AI assistant, choose the stack, do initial analysis
2. Attach the relevant template file to the discussion
3. Ask the AI to generate a bootstrap script based on the template and your discussion
4. Run the script, start working

See each template file for its specific workflow.

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
https://github.com/<you>/claude-templates/blob/v1.2/project-bootstrap.md
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
