---
title: "Y si empezamos documentando los ejemplos"
date: 2023-09-23
draft: false
categories: ["programming"]
tags: ["spanish", "docstring"]
---

## Comienza con Ejemplos

Cuando se trata de documentar código, he descubierto que documentar los
ejemplos primero es de gran ayuda. Esto podria llamarse perfectamente algo como
Desarrollo Dirigido por Ejemplos o Example Driven Development, que suena mucho
mejor en inglés, que se note que ahora trabajo en Publi ;-)

### Iteraciones fluidas

A veces comienzas a escribir el código sin saber cómo será el resultado final
(no es TDD). En esos casos, escribir un ejemplo de lo que hace tu código me ha
ahorrado mucho tiempo. Evita tener que escribir una y otra vez un fragmento de
código para probar lo que estoy haciendo.

### No todo es TDD

No siempre se puede aplicar TDD, y si
lo aplicas siempre, en algún momento te encontrarás en la situación de tener
que reescribir el test una y otra vez, o incluso tener que desecharlos por
haberlos escrito apresuradamente antes de tener clara la lógica final.

Esto es un punto medio entre no tener nada y tener tests. De hecho, si en tus
ejemplos tambien incluyes los resultados, boom! se convierten en
[doctests][doctest].

### Documentación útil

Lo que más me entusiasma de este enfoque es cómo reduce la brecha entre la
teoría y la práctica. Poner ejemplos permiten a cualquier desarrollador
entender rápidamente cómo usar mi código. Además, resaltan casos especiales que
a menudo se pasan por alto en explicaciones teóricas.

He visto mucha documentación que carece de ejemplos, lo que para mi es un
fallo. A menudo encuentro toda la explicación que motiva la existencia de
esa joya de software, pero no sé cómo usarla. Otras veces se documentan los
parámetros que resultan obvios (`arr`, `percent`, `threshold`, ...), pero no se
proporciona un ejemplo de uso que permita reducir la brecha entre la teoría y
la práctica.

## Conclusión

Comenzar con ejemplos en mis docstrings ha mejorado significativamente la
usabilidad y la mantenibilidad de mi código. Te animo a probar esta técnica y
descubrirás cómo facilita la vida de quienes trabajan con tu código.


[doctest]: https://docs.python.org/es/3/library/doctest.html
