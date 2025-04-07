---
title: "Makefile"
date: 2024-09-19
draft: false
categories: ["programming"]
tags: ["spanish", "makefile", "python"]
---


## Makefile

Some times you need a good starting point, here is mine:

```make
# variables
PYVER  := 3.10
venv   := .venv
python := $(venv)/bin/python
pip    := $(venv)/bin/pip


##@ Utility
.PHONY: help
help:  ## Display this help
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make <target>\033[36m\033[0m\n"} /^[a-zA-Z\._-]+:.*?##/ { printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)


##@ Setup
$(venv):
    @python$(PYVER) -m venv $(venv)


.PHONY: install
install: $(venv)  ## install
	$(pip) install . -r requirements.txt


##@ Development
.PHONY: dev
dev: $(venv) ## install dev mode
	$(pip) install -e .[dev]

.PHONY: test
test: $(venv) ## run tests
	$(python) -m pytest tests

.PHONY: lint
lint: $(venv)  ## run linting check
	$(python) -m ruff ./src

.PHONY: format
format: $(venv)  ## fomat code using ruff
	$(python) -m ruff format src test


.PHONY: requirements.txt
requirements.txt:  ## update requirements.txt, e.g. make requirements.txt
	@test -d /tmp/venv && rm -r /tmp/venv || true
	@$(python) -m venv /tmp/venv
	@/tmp/venv/bin/python -m pip -q install pip -U
	@/tmp/venv/bin/python -m pip -q install . --progress-bar off
	@/tmp/venv/bin/python -m pip freeze > requirements.txt

```
