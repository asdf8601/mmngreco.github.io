---
title: "Exploring uv (python package manager)"
date: 2024-11-21T17:33:03+01:00
draft: true
categories: ["programming"]
labels: ["uv", "python", "english"]
---


## Quick data

- Similar to `poetry` API
- python complaint
  - use `.python-version` like `pyenv`
  - use `pyproject.toml` like `pip`
  - use `.venv` as default
- `pyproject.toml` governed
- `uv venv` equivalente to `python -m venv .venv`
- `uv pip ...` equivalent to `pip ...`
- Easy and quick installation
  - `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - upgrade with `uv self update`

### Development

#### Common tasks we will find and use:

- Minimal scaffolding
- Manage venv / dependencies
- Manage python versions
- Build distributions


#### Example

```bash
mkdir prj
cd prj
uv init
uv venv
uv add pandas scikit-learn scipy duckdb numba numpy
uv sync
uv python install 3.11
uv python pin 3.11
```

> https://docs.astral.sh/uv/getting-started/features/
> https://docs.astral.sh/uv/guides/projects/
> https://docs.astral.sh/uv/concepts/python-versions/#adjusting-python-version-preferences
> https://docs.astral.sh/uv/guides/integration/docker/

### They move fast

They have an amazing philosophy of using built-in Python tools. An example of
this:

Previously, they used the `[tool.uv]` table inside the `pyproject.toml` like
this:

```toml
[tool.uv]
dev-dependencies = [
    "pytest>=8.3.3",
]
```

to handle dev dependencies, now (`uv 0.5.4 (c62c83c37 2024-11-20)`) they
replaced that in favor of dependency groups, becoming something like this:

```
[dependency-groups]
dev = [
    "pytest>=8.3.3",
]
```

> [!NOTE]
> `uv` adopted the PEP-735: dependency groups in pyproject.toml in only 15 days.
> https://github.com/astral-sh/uv/issues/8090

Do you realize how great this is? Before, you were only able to install dev
dependencies using `uv`, but after the change, you can use whatever tool you
want, as long as they are Python-compliant, like `pip`.

### Upgrade just one dependency

> https://docs.astral.sh/uv/guides/projects/#managing-dependencies
>
> To upgrade a package, run uv lock with the `--upgrade-package` flag:
>
> ```bash
> uv lock --upgrade-package requests
> ```
>
> The `--upgrade-package` flag will attempt to update the specified package to
> the latest compatible version, while keeping the rest of the lockfile intact.


### Running commands


> https://docs.astral.sh/uv/guides/projects/#running-commands

```bash
uv add flask
uv run -- flask run -p 3000
uv run app.py
```

### How to migrate

You probably already have:

- `pyproject.toml` or `setup.py`
  - be sure you have a `requires-python` entry or `.python-version`
- `requirements.txt`

In that case, you can use:

- `uv venv`
- `uv pip install -r requirements.txt`
- `source .venv/bin/activate`
- `python -V`

In case you want to switch to pure `uv` management:

- `uv add -r requirements.txt`
- `git add uv.lock pyproject.toml`
- `uv sync` from now on


## See Also

- https://peps.python.org/pep-0735/
