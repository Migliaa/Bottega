# Contributing to Bottega

Bottega is a **living framework**: a method plus templates. The best contributions generalize a pattern that proved useful on a real project.

## What's welcome
- **A new department recipe** (a lane archetype not covered by the five).
- **Clearer templates or docs** — especially anything that removed friction in practice.
- **A better tutorial / example** — another worked instance, or a sharper quickstart.
- **Fixes** to placeholders, broken links, or inconsistencies between docs and templates.

## What to keep in mind
- **Stay project-agnostic.** No domain specifics in `docs/`, `templates/`, or `departments/` — use placeholders (`{{...}}`) and explain them in `bottega.config.md`.
- **Preserve the vocabulary.** Use the terms in `docs/07-glossary.md` consistently (committente, manager, department, sprint, work-order, gate, INTERFACE). Distinctive terms are part of the framework's identity.
- **Templates must round-trip.** If you add a placeholder, add it to `bottega.config.md` and make sure `/bootstrap` can collect it.
- **English for the framework**; the *instances* it generates speak the committente's language.

## How
1. Open an issue describing the pattern and where it helped.
2. PR with the change. Keep each PR focused.
3. If you add files, update the relevant index (`README.md` structure table, `departments/README.md`, etc.).

## License
By contributing you agree your work is released under the repo's [MIT license](LICENSE).
