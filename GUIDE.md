# llm-scratch guide

## Layout

```text
llm-scratch/
├── pyproject.toml         # project metadata, dependencies, uv settings
├── uv.lock                # exact pinned versions (commit this)
├── .python-version        # 3.12
├── .venv/                 # virtual environment (managed by uv, don't edit)
└── src/llm_scratch/
    └── __init__.py        # package code; main() is the CLI entry point
```

## Everyday commands

| Task | Command |
| --- | --- |
| Run the entry point | `uv run llm-scratch` |
| Run any script | `uv run path/to/script.py` |
| Open a Python shell | `uv run python` |
| Add a dependency | `uv add torch` |
| Add a dev-only tool | `uv add --dev pytest` |
| Remove a dependency | `uv remove torch` |
| Sync after a pull or edit | `uv sync` |

You don't need to activate `.venv`, because `uv run` uses it automatically.

## Writing code

Put modules in `src/llm_scratch/`, for example `src/llm_scratch/tokenizer.py`, and import them as a package:

```python
from llm_scratch.tokenizer import encode
```

This works from anywhere in the project, including scripts and tests, because uv installs the package into `.venv` in editable mode. Your edits take effect right away with no reinstall.

To add a second command, list it under `[project.scripts]` in `pyproject.toml`, then run `uv sync`:

```toml
[project.scripts]
llm-scratch = "llm_scratch:main"
train = "llm_scratch.train:main"
```

After that, `uv run train` runs it.

## Tests (optional)

```bash
uv add --dev pytest
mkdir tests        # put test_*.py files here
uv run pytest
```

## Notebooks (optional)

```bash
uv add --dev jupyterlab
uv run jupyter lab
```

## Git

Commit `pyproject.toml`, `uv.lock`, `.python-version` and `src/`. The `.gitignore` already excludes `.venv/`.
