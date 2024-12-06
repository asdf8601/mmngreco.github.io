---
title: "Cambiando el Explorador Predeterminado en Linux"
date: 2024-01-29
draft: false
categories: ["programming"]
labels: ["file-manager", "thunar", "linux", "ubuntu", "howto", "spanish"]
---

### Usando el archivo `.desktop`

Lo primero: ubicar el correspondiente archivo `.desktop`:

```bash
ls /usr/share/applications/*.desktop | grep -I thunar
ls ~/.local/share/applications/*.desktop | grep -I thunar
```

Si no aparece, hay que crearlo:

```bash
[Desktop Entry]
Name=Open Folder
TryExec=thunar
Exec=thunar %U
NoDisplay=true
Terminal=false
Icon=folder-open
StartupNotify=true
Type=Application
MimeType=x-directory/gnome-default-handler;x-directory/normal;inode/directory;application/x-gnome-saved-search;
```

### Aplicación predeterminada

Luego, cambia el mimetype predeterminado:

```bash
nvim ~/.local/share/applications/defaults.list
```

Ahora, agrega esta línea (según el resultado anterior):

```ini
inode/directory=thunar.desktop
x-directory/normal=thunar.desktop
```

### Comprobación

Compueba que funciona correctamente:

```bash
xdg-open .
```

## Referencias

- https://help.ubuntu.com/community/DefaultFileManager
