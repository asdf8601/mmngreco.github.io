---
title: "Guía sencilla para iniciar un proyecto Python desde cero"
date: 2024-01-26
draft: false
categories: ["programming"]
tags: ["python", "pyproject", "build", "dependencies"]
---

Mira esta ejemplo de cómo configurar un proyecto Python con lo esencial para
comenzar rápidamente, pero con la capacidad de ajustar y escalar conforme las
necesidades aumenten.

- `pyproject.toml`: archivo de configuración estándar [PEP-518][pep518] para
  proyectos Python.
- `Makefile`: Se utiliza para automatizar tareas como testing, build, installs,
  entre otros.
- `__version__` (opcional): Es útil para mantener un registro de las distintas
  versiones de tu proyecto.

Mi estrategia favorita es comenzar con todos los elementos en un solo archivo y
luego, a medida que el proyecto crezca, dividirlos en la estructura de carpetas
basada en `src`. No me preocuparia por esta reestructuración hasta que sea
verdaderamente necesario.

Aqui uso `setuptools-scm` para obtener una versión automática de la librería
basada en los tags de Git, los cuales deben seguir el estándar [PEP-440][pep440].


## Ejemplo: Creando `takt`

Esto es lo que use para crear [`takt`][takt], con el que hasta ahora estoy
muy contento:

```bash
mkdir -p takt/tests
touch takt/README.md
touch takt/Makefile
touch takt/pyproject.toml
touch takt/takt.py
```

Esto es lo minimo que se necesita para empezar un proyecto python.

## `pyproject.toml`

Este archivo es una especificación moderna de empaque para proyectos de Python,
según [PEP-518][pep518]. Sirve para declarar las dependencias que necesitas
para construir tu proyecto desde la fuente. Esta es una alternativa al uso de
`setup.py` y `requirements.txt` en proyectos Python.

`pyproject.toml` es como un archivo de configuración centralizado para muchas
de las herramientas que podrías usar en tu proyecto Python. Su uso es cada vez
más común a medida que dichas herramientas migran su soporte al archivo
`pyproject.toml`.


```toml
[build-system]
requires = ["setuptools>=60", "setuptools-scm>=8.0"]
build-backend = "setuptools.build_meta"

[project]
name = "takt"
dynamic = ["version"]
authors = [{name="Max Greco", email="asdf0x2199@gmail.com"}]
readme = "README.md"
requires-python = ">=3.6"
dependencies = ["rich", "typer", "pandas"]
license = {file = "LICENSE"}
description = "Takt is a CLI tool for tracking time."


[project.scripts]
takt = "takt:app"


[tool.setuptools_scm]
```


## Definición de `__version__` (Opcional)

Asignar un número de versión o `__version__` a un proyecto es un paso opcional,
pero definitivamente beneficioso. Lo que hace es facilitar la identificación
del número de versión de la biblioteca, sin la necesidad de generar un archivo
adicional. Por supuesto, si se prefiere seguir esta ruta, se puede. Simplemente
es una opción más en el amplio espectro de posibilidades que el desarrollo
proporciona.

```python
from importlib.metadata import version, PackageNotFoundError

try:
    __version__ = version("package-name")
except PackageNotFoundError:
    # package is not installed
    pass
```


## `Makefile`

Este `Makefile` es una joya, viene autodocumentado por lo que no hay que
ocuparse de mantener un help actualizado. Tomate tu tiempo para leerlo y
entenderlo, es bastante facil.


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
	@if [ -x "`command -v conda`" ]; then \
		conda create --prefix $(venv) python=$(PYVER) -y -q; \
	else \
		python$(PYVER) -m venv $(venv); \
	fi


.PHONY: install
install: $(venv)  ## install
	$(pip) install . -r requirements.txt


.PHONY: push
push: tag ## push to origin a new tag, e.g. make push v=<version>
	git push origin main
	git push --tags


##@ Development
.PHONY: dev
dev: $(venv) ## install dev mode
	$(pip) install -e . -r requirements-dev.txt

.PHONY: test
test: $(venv) ## run tests
	@$(pip) -q install pytest
	$(python) -m pytest tests

.PHONY: lint
lint: $(venv)  ## run linting check
	@$(pip) -q install ruff
	$(python) -m ruff ./src

.PHONY: black
black: $(venv)  ## apply black to source code
	@$(pip) -q install black
	$(python) -m black -l79


.PHONY: requirements.txt
requirements.txt:  ## generate requirements.txt, e.g. make requirements.txt
	@test -d /tmp/venv && rm -r /tmp/venv || true
	@$(python) -m venv /tmp/venv
	@/tmp/venv/bin/python -m pip -q install pip -U
	@/tmp/venv/bin/python -m pip -q install . --progress-bar off
	@/tmp/venv/bin/python -m pip freeze > requirements.txt
	$(MAKE) fix-requirements.txt

.PHONY: fix-requirements.txt
fix-requirements.txt:  ## fix requirements.txt using GH_TOKEN variable for privates repos.
	@if [ "$(shell uname -s)" = "Linux" ]; then \
		sed -i 's/git+ssh:\/\/git@/git+https:\/\/$${GH_TOKEN}@/' requirements.txt; \
		sed -i '/file:/d' requirements.txt; \
	elif [ "$(shell uname -s)" = "Darwin" ]; then \
		sed -i '' -e 's/git+ssh:\/\/git@/git+https:\/\/$${GH_TOKEN}@/' requirements.txt; \
		sed -i '' -e '/file:/d' requirements.txt; \
	fi
	@cat requirements.txt
```



[pep518]: https://peps.python.org/pep-0518/
[pep440]: https://peps.python.org/pep-0440/
[tatk]: https://github.com/asdf0x2199/takt
