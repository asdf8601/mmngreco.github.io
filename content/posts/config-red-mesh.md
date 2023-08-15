---
title: "Configurando red mesh usando 2 routers AX3600"
date: 2023-08-15
draft: false
categories: ["hacking"]
labels: ["red", "xiaomi", "router", "mesh", "ax3600"]
---

No se cuantas horas he desper... invertido en tratar de configurar la red mesh
pero hoy por fin lo he conseguido. Aqui dejo los pasos que he seguido para
abrazar el exito en esta empresa.

## La receta


> Fuente original: https://xiaomi.eu/community/threads/ax-3600-mesh-not-detected.58886/post-611886

1. Todo desconectado
2. Enchufa solo la electricidad y Reseteo de fabrica del router (ambos).
3. Cuando este listo enchufa el ethernet
4. Configura el router usando la red 5g (siempre 5g)
  1. Conectarse al wifi
  2. Esperar a que salga el dialogo de configuracion.
  3. Desconectar del wifi y desactivar wifi del dispositivo usado durante ~30seg
1. Conectar el router reset de fabrica a la electricidad
1. Cuando las luces indican que esta iniciado enchufa el ethernet (luces naranjas y azules)
1. Busca la red del router Xioami con el nombre 5G
1. Espera a que aparezca una ventana emergente en el navegador de MiWifi.
1. Configura el WIFI añadiendo contraseña.
  1. Contraseña: `12345678`
  1. Usa la misma contraseña de Wifi para la contraseña del router. Esto es asi
     por defecto, asi que no cambies nada.
1. Apaga el wifi en el ordenador durante 30 segundos. ¯\\_ (ツ) \_/¯
1. Selecciona la nueva red `12345678_5G` (!!!) ¡Siempre usa la red 5G!
1. Asegúrate de que el segundo router esté restablecido de fábrica y cerca del
   Nodo Maestro (el primer AX3600) y conectado por ethernet al nodo principal.
1. Usa el script proporcionado por ShotokanZH. ¡Gracias de nuevo!
2. Terminal:
    1. `git clone https://github.com/ShotokanZH/MiWiFi_Mesh_Node_Adder`
    2. `python3 -m venv venv`
    3. `source venv/bin/activate`
    4. `python3 -m pip install -r requirements.txt`
    5. `python3 addmesh.py`
      1. Introduce `192.168.31.1` (IP del AX3600 Maestro)
      1. Contraseña: `12345678`
      1. El script te mostrará la dirección MAC correcta, asegúrate de copiar la de
         5G y pegarla donde se pida. ¡Presiona enter, espera al reinicio!


### Notas:

1. La conseña es de ejemplo.
1. No uses la dirección MAC que aparece en la etiqueta del router.

## Links

- https://xiaomi.eu/community/threads/ax-3600-mesh-not-detected.58886/
- https://github.com/ShotokanZH/MiWiFi_Mesh_Node_Adder
