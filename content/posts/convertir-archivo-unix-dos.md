---
title: "Cambiando el formato de un arhivo."
date: 2024-04-13
draft: false
categories: ['programming']
tags: ['unix', 'neovim', 'bash', 'scp']
---

Me encontraba trabajando tranquilamente cuando tuve la necesidad de copiar un
archivo de un servidor a mi maquina local para continuar trabajando, asi que me
dispuse a ejecutar `scp <server> ~/file.sh` y al ejecutarlo me encuentro con
este error:


```bash
$ bash ./file.sh
./file.sh: line 2: $'\r': command not found
 ")60)) * 5rror: invalid arithmetic operator (error token is "
./file.sh: line 6: $'\r': command not found
./file.sh: line 7: syntax error near unexpected token `$'{\r''
'/file.sh: line 7: `show_progress() {
```


## Solución


Me llevo unos minutos darme cuenta de que el problema venia por el formato del
archivo `file.sh` y no por la sintaxis.


En Neovim, es muy fácil cambiar el formato. Basta con ejecutar `:set ff=unix`,
guardar, y voilá.
