---
title: "Optimización de zsh y nvm en macOS"
date: 2024-02-02
draft: false
categories: ["programming"]
labels: ["macOS", "nvm", "zsh", "profiling", "spanish"]
---

## Haciendo profiling de zsh

El primer paso es hacer profiling del archivo `.zshrc`. Para hacer el
profiling, incluye lo siguiente en el archivo de configuración:

```zsh
zmodload zsh/zprof
# Tu configuración existente va aquí
zprof
```

Para ejecutar el profiling, utiliza el siguiente comando:

```shell
time zsh -i -c exit
```

## Mejorar los tiempos de carga de `nvm`

Para mejorar los tiempos de carga de `nvm`, realiza los siguientes cambios en
tu archivo `.zshrc`:

```diff
--- a/macos/home/.zshrc
+++ b/macos/home/.zshrc
@@ -1,17 +1,23 @@
 #!/usr/bin/env zsh
+
+zstyle ':omz:plugins:nvm' lazy yes
 export ...
+
+plugins=(
+  nvm
+)
 source $ZSH/oh-my-zsh.sh
 source $DOTFILES_UBUNTU/home/.prompt.zsh
@@ -57,9 +63,10 @@ command -v pyenv > /dev/null 2>&1 && eval "$(pyenv init -)" 2> /dev/null
 [ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
 # source /usr/share/doc/fzf/examples/key-bindings.zsh

-export NVM_DIR="$HOME/.nvm"
-[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # Esto carga nvm
-[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # Esto carga nvm bash_completion
+

```

Fuente: [https://dev.to/thraizz/fix-slow-zsh-startup-due-to-nvm-408k](https://dev.to/thraizz/fix-slow-zsh-startup-due-to-nvm-408k)
