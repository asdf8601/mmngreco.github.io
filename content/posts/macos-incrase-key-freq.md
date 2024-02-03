---
title: "Acelerando la repeticion de teclas en MacOS"
date: 2024-02-03
draft: false
categories: ["programming"]
labels: ['macos', 'hacking', 'spanish', 'keyboard', 'keys']
---


Aumentar la frecuencia de repetición de las teclas puede mejorar la eficiencia
al introducir largas secuencias de caracteres repetidos. Buscando, he
conseguido encontrar cómo aumentar la frecuencia de repetición de teclas
manteniendo pulsada una tecla, esto es útil para introducir espacios o moverse
con las flechas.

```bash
#!/bin/bash
# Define los ajustes de repetición de tecla
defaults write -g InitialKeyRepeat -int 16
defaults write -g KeyRepeat -int 1
# Deshabilita la función de presionar y mantener las teclas
defaults write -g ApplePressAndHoldEnabled -bool false

# Fuente
# https://gist.github.com/hofmannsven/ff21749b0e6afc50da458bebbd9989c5
# Para simular la pulsación de teclas
# https://mac-key-repeat.zaymon.dev/
```
Las configuraciones del script se pueden ajustar para optimizar aún más la
experiencia de usuario basándose en necesidades individuales.
