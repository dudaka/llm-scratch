# llm-scratch guide

## Layout

```text
llm-scratch/
├── pyproject.toml         # project metadata, dependencies, uv settings
├── uv.lock                # exact pinned versions (commit this)
├── .python-version        # 3.13
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

## Notebooks

Step-by-step experiments live in `notebooks/`, in learning order:

| Notebook | Covers |
| --- | --- |
| `tokenizing-text.ipynb` | Download `the-verdict.txt` and split it into tokens with a regex |
| `tokens-2-ids.ipynb` | Build a vocabulary, map tokens to IDs, `SimpleTokenizerV1`/`V2` with `<\|unk\|>` and `<\|endoftext\|>` |
| `byte-pair-encoding.ipynb` | GPT-2 byte-pair encoding with `tiktoken` |

`the-verdict.txt` is the sample text. The notebooks open it by relative path, so run them with `notebooks/` as the working directory (the default in VS Code and Jupyter).

Notebooks need a kernel:

```bash
uv add --dev ipykernel     # for VS Code: select the .venv interpreter as the kernel
uv add --dev jupyterlab    # optional, for the browser UI
uv run jupyter lab
```

## Git

Commit `pyproject.toml`, `uv.lock`, `.python-version` and `src/`. The `.gitignore` already excludes `.venv/`.
