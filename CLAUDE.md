# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A teaching artifact, not an application. The deliverable is a 3.5-hour
hands-on workshop ("Writing Pythonic Code"), usable both live (PyCon /
PyTexas tutorial slot) and self-paced (the same Jupyter notebooks open in
Google Colab). There is no build, no test suite, and no runtime in the
traditional sense. The notebooks themselves are the runtime, and "passing"
means cells execute top-to-bottom in a fresh kernel and produce the
documented outputs.

## Architecture

Everything is organized around **Performance-Based Learning (PBL)**. The
chain is enforced top-down:

```
program outcome → competency → learning objective → learning activity → performance assessment
```

`course-plan.md` is the source of truth for that chain. Each of the five
modules has competencies, objectives, an assessment, and a numbered
activity table. **Every code/markdown cell in a `Content/` notebook should
trace back to an activity row in `course-plan.md`. Every exercise in
`Exercises/Practice/` should map 1:1 to a Solution and to the performance
criteria of its module's competency.**

Three parallel artifact families:

- `Content/0N-*.ipynb` — narrated module notebooks. Source of truth for the
  Slidev deck that lives in `../../MasonEgger/presentations/workshops/writing-pythonic-code/`.
- `Exercises/Practice/0N-*.ipynb` — what the learner attempts during the
  module. Includes scaffolds, success criteria at the top, and TODOs.
- `Exercises/Solution/0N-*.ipynb` — reference answers. Structurally
  parallel to the Practice scaffolds (e.g. if Practice uses
  `def wrapper(*args, **kwargs):`, Solution should too).

If you change a Practice notebook, check the matching Solution. If you
change either, check `course-plan.md`'s assessment criteria still describe
the exercise accurately.

## Pedagogical patterns to preserve

These are deliberate stylistic choices, documented under "Course Style"
in `course-plan.md`. Don't sand them off:

- **Code sandwich.** Every code block shown on a slide exists as an
  executable cell in the matching `Content/` notebook. Slides narrate;
  notebooks provide the runtime.
- **Recognition openers.** Each module starts with a snippet the learner
  has likely seen in the wild (`@app.route`, `with open(...)`, etc.)
  before introducing mechanics.
- **Problem-driven mechanics.** Closures are taught as a *response* to a
  posed problem (a counter needing persistent state), not as an isolated
  language feature.
- **Mid-section detours with Monty Python flavor.** "And now for something
  completely different..." segues out of decorators to teach scoping,
  then back. Voice has personality on purpose.
- **Show-and-fail antipatterns.** When teaching `__call__`, the course
  demonstrates the feature, then reflects on why a Pythonista wouldn't
  reach for it. Don't strip the reflection.
- **Explicit success criteria.** Every exercise slide and every Practice
  notebook states the bar before the learner attempts ("You'll know
  you've succeeded when...").

## Prose style

**No em-dashes (`—`) in flowing prose.** This applies to anything new you
add to notebook markdown cells, slide chapters, and `.md` files. Replace
with a colon, comma, period, or parentheses depending on context.
Em-dashes are allowed in table cells, headings, structural labels like
`**Module N — Topic.**`, and "label — value" bullet patterns.

**No forward references.** Every section should stand on its own. Don't
write "as we'll see later", "this is the foundation of decorators in the
next module", or similar pointers to upcoming material. If a concept
genuinely depends on something earlier, restate or link backward — but
forward pointers are a no-go. Applies to both notebook cells and slide
chapters.

## Slide-side deviations and sync-back

The Slidev deck in
`../../MasonEgger/presentations/workshops/writing-pythonic-code/` is
mostly a 1:1 reflection of the notebooks, but the deck occasionally
diverges on purpose — images instead of tables for projector legibility,
bullets instead of paragraphs for slide density, narrative prompts that
only make sense when spoken aloud. Each intentional divergence is
annotated in the slide chapter with an inline HTML comment:

```md
<!-- slide-only: image reads better on a projector than the notebook's table -->
```

When you re-run the notebook→slide sync, **don't clobber anything that
sits directly below a `<!-- slide-only -->` marker** even if the
notebook prose differs.

The reverse list — places where the slide is ahead of the notebook and
the notebook should catch up — lives in `prune.md` in this repo, under
the "Sync-back edits" section. Work that list before the next sync and
the markers will go away naturally.

## Common tasks

### Convert a notebook to markdown (for slide sync)

The Slidev deck is updated by converting `Content/` notebooks to markdown,
then pulling the converted content into the slide chapters. `jupyter` /
`nbconvert` are not installed locally; run them one-off with `uvx`:

**Single file:**

```bash
uvx --from nbconvert jupyter-nbconvert --to markdown Content/01-First-Class-Functions.ipynb
```

**All five Content notebooks at once:**

```bash
uvx --from nbconvert jupyter-nbconvert --to markdown Content/*.ipynb
```

Output `.md` files land next to the source `.ipynb` (e.g.
`Content/01-First-Class-Functions.md`). From there, the slide chapters in
`../../MasonEgger/presentations/workshops/writing-pythonic-code/chapters/`
get updated manually.

The generated `.md` files are intermediate artifacts. Delete them after
syncing the slides, or add `Content/*.md` to `.gitignore` to keep them out
of commits. They are not the source of truth; the `.ipynb` files are.

Notes:

- `--from nbconvert` is required because the package is `nbconvert` but
  the CLI entry point is `jupyter-nbconvert`. Without `--from`, `uvx`
  looks for a package literally named `jupyter-nbconvert` and fails.
- `uvx` provisions an ephemeral environment with `nbconvert` and its
  dependencies (~31 packages, ~40ms after the first run). Nothing
  persists; nothing pollutes your global Python.
- Only `Content/` notebooks feed the slides. Do not convert
  `Exercises/Practice/` or `Exercises/Solution/` notebooks for slide sync.

### Edit a notebook safely from the command line

`.ipynb` files are minified JSON. Editing them as text risks corruption.
Use a small Python script with the `json` module: load, mutate the
`cells` array, write back with `json.dumps(nb, separators=(",", ":"), ensure_ascii=False)`
to preserve the existing minified style. When inserting new cells, build
them as dicts with `cell_type`, `source` (list of lines, each ending in
`\n` except possibly the last), `metadata: {}`, and for code cells
`outputs: []` and `execution_count: None`.

### Verify a notebook before delivery

There's no automated test. The verification is: open in Colab (or `jupyter
lab` locally), Run All from a fresh kernel, confirm every cell executes
without unintended errors and outputs match the inline `# Output:`
comments. Cells in Module 2 deliberately raise `NameError` / `UnboundLocalError`
as part of the scoping demo. Those tracebacks are expected.

## Git workflow

- `commit-msg.md` is gitignored. Never stage it.
- `.claude/settings.local.json` is local-only.
- Workshop content is the deliverable; treat `main` as a public branch
  even though it's the only one in active use.
