---
title: "UV Review: The Quirkiest Python Package Manager Around"
date: 2024-11-21T17:33:03+01:00
draft: false
categories: ["programming"]
labels: ["uv", "python", "english"]
---


## Quick facts

- Similar to `poetry` API
- Python compliant
  - Uses `.python-version` like `pyenv`
  - Uses `pyproject.toml` like `pip`
  - Uses `.venv` as default
- `pyproject.toml` governed
- `uv venv` equivalent to `python -m venv .venv`
- `uv pip ...` equivalent to `pip ...`
- Easy and quick installation
  - `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Upgrade with `uv self update`


![screenshot comparing poetry and uvmage-1.jpg](https://i.postimg.cc/7Y1SX12p/image-1.jpg)

> Note:
> Screenshot comparing `poetry` (top) and `uv` (down). You can see the
> different implementations of the `pyproject.toml` and the performance (15s vs
> 1s).

## Development

### Common tasks we will find and use:

- Minimal scaffolding
- Manage venv/dependencies
- Manage Python versions
- Build distributions


### Example

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

Resources:

- https://docs.astral.sh/uv/getting-started/features/
- https://docs.astral.sh/uv/guides/projects/
- https://docs.astral.sh/uv/concepts/python-versions/#adjusting-python-version-preferences
- https://docs.astral.sh/uv/guides/integration/docker/

### They Move Fast and Stick to the Standard

They have an amazing philosophy of using built-in Python tools. An example of
this, previously, they used the `[tool.uv]` table inside the `pyproject.toml`
like this:

```toml
[tool.uv]
dev-dependencies = [
    "pytest>=8.3.3",
]
```

to handle dev dependencies, now (`uv 0.5.4 (c62c83c37 2024-11-20)`) they
replaced that in favor of dependency groups, becoming something like this:

```toml
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

### How to Upgrade Only One Dependency

> https://docs.astral.sh/uv/guides/projects/#managing-dependencies
>
> To upgrade a package, run uv lock with the `--upgrade-package` flag:
>
>
> `uv lock --upgrade-package requests`
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

## How to migrate

You probably already have most of the files needed and use `pip`:

- `setup.py`
- `pyproject.toml`
  - be sure you have a `requires-python` entry or `.python-version`
- `requirements.txt`

> I would recommend fully migrate `setup.py` to `pyproject.toml`.

In that case, you can use the following commands:

- `uv venv`
- `uv pip install -r requirements.txt`
- `source .venv/bin/activate`
- `python -V`

In case you want to switch to pure `uv` management you can try this:

- `uv add -r requirements.txt`
- `git add uv.lock pyproject.toml`
- `uv sync` from now on

To move from `poetry` to `uv`, use the `requirements.txt` standard to migrate
dependencies:

- `poetry export --without-hashes -o requirements.txt`
- `uv add -r requirements.txt`
- `poetry export --dev --without-hashes -o requirements-dev.txt`
- `uv add -dev -r requirements-dev.txt`
- `git add uv.lock pyproject.toml`
- remove poetry tables from `pyproject.toml`


## See Also

- https://peps.python.org/pep-0735/
- https://docs.astral.sh/uv/getting-started/features/
- https://docs.astral.sh/uv/guides/projects/
- https://docs.astral.sh/uv/concepts/python-versions/#adjusting-python-version-preferences
- https://docs.astral.sh/uv/guides/integration/docker/
