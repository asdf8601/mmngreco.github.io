---
title: "Crea projectos python minimalistas con un comando"
date: 2024-02-25
draft: false
categories: ["programming"]
labels: ["python", "programming", "spanish"]
---


Basándome en mi [anterior post][prev-post] sobre cómo crear proyectos Python
minimalistas, me encontré copiando y pegando con bastante frecuencia. Echaba de
menos algo que me permitiera hacer este trabajo (que ya de por sí es bastante
sencillo) aún más sencillo.

[prev-post]: https://mmngreco.dev/posts/python-project-scratch/

## Punto de entrada

He comenzado a trabajar en una prueba de concepto para ver si funciona. Lo
primero que debo hacer es definir el punto de entrada, que en mi caso será
este:

```bash
# Usage:
# $ py-here name
py-here() {
    mkdir -p  $1/{tests,docs}
    touch $1/{Makefile,pyproject.toml,$1.py}
    # TODO add more
}
```

## pyproject

Ya tenemos un mínimo bastante aceptable con eso, pero podemos mejorarlo si
incluimos el `pyproject.toml` a partir de una plantilla. Tomé el del post
anterior y lo modifiqué para incluir la variable `$name`, permitiendo así
convertirlo en una plantilla de uso común.

```bash
[build-system]
requires = ["setuptools>=60", "setuptools-scm>=8.0"]
build-backend = "setuptools.build_meta"

[project]
name = "${name}"
dynamic = ["version"]
authors = [{name="Max Greco", email="mmngreco@gmail.com"}]
readme = "README.md"
requires-python = ">=3.6"
dependencies = ["rich", "typer", "pandas"]
license = {file = "LICENSE"}
description = "${name} is a CLI tool for tracking time."


[project.scripts]
${name} = "${name}:app"


[tool.setuptools_scm]
```


Voy a guardar esto en un [gist][gist] para poder accederlo desde cualquier
lugar y mantenerlo actualizado. Después, haré lo mismo con el [Makefile][make].


## Haciendo pruebas

Aquí una prueba de concepto. La idea es probar que el `curl` funciona y consigo
reemplazar la variable con `sed`.

```bash
$ name=yayay && curl -sSL https://gist.githubusercontent.com/mmngreco/2a371093fcb704fbff771e39479e75dc/raw/pyproject.toml  | sed "s/\${name}/${name}/g"
[build-system]
requires = ["setuptools>=60", "setuptools-scm>=8.0"]
build-backend = "setuptools.build_meta"

[project]
name = "yayay"
dynamic = ["version"]
authors = [{name="Max Greco", email="mmngreco@gmail.com"}]
readme = "README.md"
requires-python = ">=3.6"
dependencies = ["rich", "typer", "pandas"]
license = {file = "LICENSE"}
description = "yayay is a CLI tool for tracking time."


[project.scripts]
yayay = "yayay:app"


[tool.setuptools_scm]
```

## Envolviendo todo en un comando

Y ahora tenemos un comando que hace lo que quiero. Voy a envolver todo en la
función bash anterior y veamos el resultado final. Para ello, incluye esto en
tu `.zshrc` o `.bashrc`:

```bash
# Usage:
# $ py-here name
py-here() {
    name=$1
    mkdir -p  ${name}/{tests,docs}
    touch ${name}/{Makefile,pyproject.toml,${name}.py}
    # you can always make the following steps optionals
    curl -sSL https://gist.githubusercontent.com/mmngreco/2a371093fcb704fbff771e39479e75dc/raw/pyproject.toml  | sed "s/\${name}/${name}/g" > ${name}/pyproject.toml
    curl -sSL https://gist.githubusercontent.com/mmngreco/2a371093fcb704fbff771e39479e75dc/raw/Makefile > ${name}/Makefile
    curl -L -s https://www.gitignore.io/api/python > ${name}/.gitignore
    cd ${name} && git init && git add . && git commit -m "add files"
}
```

## Y para terminar, asi es como se ve

Abre una nueva consola y observa el resultado:

```bash
$ py-here newProject
newProject
newProject/tests
newProject/docs
Initialized empty Git repository in /private/tmp/newProject/.git/
[main (root-commit) 92821f0] add files
 4 files changed, 269 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 Makefile
 create mode 100644 newProject.py
 create mode 100644 pyproject.toml
```

```bash
$ ls -la
total 24
drwxr-xr-x  12 mgreco  wheel   384B Feb 25 17:26 .git
-rw-r--r--   1 mgreco  wheel   3.4K Feb 25 17:26 .gitignore
-rw-r--r--   1 mgreco  wheel   2.0K Feb 25 17:26 Makefile
drwxr-xr-x   2 mgreco  wheel    64B Feb 25 17:26 docs
-rw-r--r--   1 mgreco  wheel     0B Feb 25 17:26 newProject.py
-rw-r--r--   1 mgreco  wheel   473B Feb 25 17:26 pyproject.toml
drwxr-xr-x   2 mgreco  wheel    64B Feb 25 17:26 tests
```

```bash
$ cat pyproject.toml
[build-system]
requires = ["setuptools>=60", "setuptools-scm>=8.0"]
build-backend = "setuptools.build_meta"

[project]
name = "newProject"
dynamic = ["version"]
authors = [{name="Max Greco", email="mmngreco@gmail.com"}]
readme = "README.md"
requires-python = ">=3.6"
dependencies = ["rich", "typer", "pandas"]
license = {file = "LICENSE"}
description = "newProject is a CLI tool for tracking time."


[project.scripts]
newProject = "newProject:app"


[tool.setuptools_scm]
```


Boom! Happy coding!


[gist]: https://gist.githubusercontent.com/mmngreco/2a371093fcb704fbff771e39479e75dc/raw/pyproject.toml
[make]: https://gist.githubusercontent.com/mmngreco/2a371093fcb704fbff771e39479e75dc/raw/Makefile
