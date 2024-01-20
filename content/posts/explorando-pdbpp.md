---
title: "Explorando pdb++"
date: 2023-09-03
draft: false
categories: ["programming"]
labels: ["Python", "pdbpp", "Debugging", "Spanish", "Hacking"]

---


## PDB++ es una joya!

[`pdbpp`][repo] es una alternativa avanzada al depurador `pdb` incorporado en
Python. Es una extensión de `pdb` que agrega varias características útiles para
la depuración de código Python, como el resaltado de sintaxis, autocompletado,
modo interactivo (`sticky`) y mucho mas.

```bash
pip install pdbpp
```


## Breakpoints


### Codigo

Pon una de estas lineas donde quieras parar la ejecución:

```python
# foo.py


def main():
  avar = 100
  bvar = 200
  import pdb; pdb.set_trace()
  # or
  breakpoint()
  out = avar + bvar
  return out
```

Para depurar, ejeucta:`python foo.py`

### Interactivamente

Puedes usar el modulo `pdb` directamente: `python -m pdb foo.py`

```
(Pdb++) b <file>/<function>/<class>:<lineno>, <condition>
(Pdb++) b foo:6
(Pdb++) h # or help
(Pdb++) c # or continue
```


### Usando `.pdbrc`

`pdbpp` lee automaticamente el archivo `.pdbrc` si existe lo que es muy comodo
para tener tus `breakpoints` explicitos en un fichero y evitar modificar el
codigo o tener que escribirlos cada vez en la consola:

`nvim .pdbrc`:

```
# .pdbrc
print("Hello from {__file__}")
```


Ejecuta: `python foo.py`


Esto permite tener un fichero por proyecto estandarizando el debugging,
evitando por ejemplo modificar el codigo si no quieres. Compartiendo ese
fichero otros pueden debuggear siguiendo el mismo patrón, etc.

> También es posible definir un `.pdbrc` global creando un `~/.pdbrc`. Todos
> los proyectos.

## Configuración

Es posible cambiar la [configuracion por defecto de pdbpp][config]:

> [default `.pdbrc.py`][pdbrc.py]

`nvim ~/.pdbrc.py`

```python
import pdb


class Config(pdb.DefaultConfig):
    # default
    stdin_paste = 'epaste'
    filename_color = pdb.Color.lightgray
    use_terminal256formatter = False

    # custom
    prompt = ">>> "                        # Ready to copy as an example
    editor = 'nvim'                        # I love it, btw
    sticky_by_default = True               # Always shows the current long line first
    line_number_color = pdb.Color.darkred  # Use red line numbers
    truncate_long_lines = True             # Avoid ugly line wraps...
    enable_hidden_frames = True            # Show hidden frames by default
    exec_if_unfocused = None               # TODO: this is broken, open a issue.

    def setup(self, pdb):
        Pdb = pdb.__class__
        # Pdb.do_l = Pdb.do_longlist       # keep l as list (not long)
        Pdb.do_st = Pdb.do_sticky
```

## Features favoritas

### Función `pdb.disable()`

Omite los breakpoints que tengas en el codigo o en `.pdbrc`:

```python
# .pdbrc / interectivamente
import pdb; pdb.disable()
```


### Comando `edit`


En modo intercativo:

```
edit foo
```

Abre `foo` usando `$EDITOR`, require reiniciar el debugger para que tenga
efecto.


### Comando `display`

En modo intercativo / `.pdbrc`

```
display a
```

Cada vez que la variable `a` cambie se imprime el valor. Esto depende del scope
en el que es ejecutado.

### Decorador `@pdb.break_on_setattr(attrname)`


Breakpoint cuando cambia el atributo de una clase

```python
# taken from https://github.com/pdbpp/pdbpp/tree/e1c2e347cc55a6dd89e058e56a1366ada68884bc
@break_on_setattr('bar')
class Foo(object):
    pass
f = Foo()
f.bar = 42    # the program breaks here
```

Esto es muy bueno!

### Función `pdb.xpm()`

Extended post mortem:

```python
try:
  foo() # raise an error
except Exception as e:
  pdb.xpm()
```

No he conseguido que funcione, pero me encanta la idea. A ver si consigo
actualizar esto mas adelante.


## Streaming

Este post es un resumen de lo que fui explorando en streaming:

{{< youtube VGUsipiv1FA >}}

<!-- links -->
[repo]: https://github.com/pdbpp/pdbpp
[config]: https://github.com/pdbpp/pdbpp/tree/master#configuration-and-customization
[pdbrc.py]: https://github.com/pdbpp/pdbpp/blob/master/pdbrc.py

