---
title: "Makefile uv based"
date: 2024-11-25
draft: false
categories: ["programming"]
labels: ["english", "uv", "makefile"]
---

Hey there! 👋

This is a revisited version of my previous post about Makefiles, but this time
I'm using uv (the shiny new Python package manager that's faster than a
caffeinated cheetah! 🐆).

I was tired of typing the same commands over and over in my Python projects, so
I made this super cool Makefile using uv.

Feel free to steal it - I mean, borrow it - for your own projects. It's like
having a robot assistant that remembers all those pesky commands for you! 🤖

## Makefile uv based

```make
##@ Utility
.PHONY: help
help:  ## Display this help
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make <target>\033[36m\033[0m\n"} /^[a-zA-Z_-]+:.*?##/ { printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)


.PHONY: venv
uv:  ## Install uv if it's not present.
	@command -v uv >/dev/null 2>&1 || curl -LsSf https://astral.sh/uv/install.sh | sh

.PHONY: dev
dev: uv ## Install dev dependencies
	uv sync --dev

.PHONY: install
install: uv ## Install dependencies
	uv sync

.PHONY: test
test:  ## Run tests
	uv run pytest

.PHONY: lint
lint:  ## Run linters
	uv run ruff check ./src ./tests

.PHONY: fix
fix:  ## Fix lint errors
	uv run ruff check ./src ./tests --fix

.PHONY: cov
cov: ## Run tests with coverage
	uv run pytest --cov=src --cov-report=term-missing

.PHONY: doc
doc:  ## Build documentation
	cd docs && uv run make html

.PHONY: build
build:  ## Build package
	uv build
```