# Writing Pythonic Code: Features That Make Python Powerful

A 3.5-hour hands-on tutorial that takes Python developers past fundamentals into idiomatic, Pythonic feature use. Covers first-class functions, decorators and closures, comprehensions, dunder methods and operator overloading, and context managers. Inspired by *Fluent Python* (Ramalho).

Originally delivered as **Python 102: Beyond the Basics** at PyTexas 2024; retitled for PyCon US 2026.

## Contents

- `Content/` — five Jupyter notebooks, one per module. The source of truth for the workshop.
- `Exercises/Practice/` — hands-on exercises learners complete during each module.
- `Exercises/Solution/` — reference solutions.
- `course-plan.md` — Performance-Based Learning course plan: competencies, learning objectives, assessments, activities, and module sequence.

## Running the Notebooks

The fastest path is to open the notebooks in [Google Colab](https://colab.research.google.com/): no local setup, and Colab provides Python 3.12 (which is what the workshop targets).

To run locally, [`uv`](https://docs.astral.sh/uv/) is the recommended toolchain. The workshop uses only the Python standard library, so no packages need to be installed beyond Jupyter itself.

### Quick start with uv

Install `uv` (see the [uv install guide](https://docs.astral.sh/uv/getting-started/installation/) for platform-specific options):

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Then, from the repo root, run Jupyter Lab without any project setup:

```sh
uv tool run jupyter lab    # or: uvx jupyter lab
```

Lab opens at `http://localhost:8888/lab`. Navigate to `Content/01-First-Class-Functions.ipynb` to start the first module, or to `Exercises/Practice/` for the exercises.

### Alternative: dedicated virtual environment

If you'd rather have an explicit virtual environment for the workshop:

```sh
uv venv --seed --python 3.12
uv pip install jupyterlab
.venv/bin/jupyter lab
```

### Installing additional packages on the fly

The workshop is stdlib-only, but if you want to experiment with a package while working through the notebooks, you can install one without leaving Jupyter:

```python
!uv pip install pydantic   # ephemeral install into the running environment
```

See the [uv Jupyter integration guide](https://docs.astral.sh/uv/guides/integration/jupyter/) for project-mode kernels, VS Code setup, and other workflows.

## Format

Hybrid: usable as a live 3.5-hour workshop *and* fully self-paced. Notebooks contain every code block shown on the slides, so a self-paced learner gets the same content as a live attendee.

## Audience

Working Python developers ("some experience") who are comfortable with functions, classes, and loops but haven't yet internalized the language's higher-order idioms.

## Deliveries

- **PyTexas 2024** — April 19, 2024, Austin
- **PyCon US 2026** — May 14, 2026, Long Beach
