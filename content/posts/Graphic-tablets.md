---
title: "Graphic Tablets"
date: 2023-04-16T14:45:04Z
draft: false
categories: ["programming"]
labels: ["hacking", "hardware", "graphic-tablets", "display", "monitor", "fzf"]
---


## Using xsetwacom

Here you will find anything you need to know about Graphics tablet:
- https://wiki.archlinux.org/title/Graphics_tablet


What I'm using is:

```bash
apt install xsetwacom
xsetwacom -h
```

##  Common usages


```bash
$ xsetwacom set <stylus-PID> MapToOutput <display>
$ # the smart version
$ xsetwacom set $(xsetwacom list | grep "stylus" | awk '{print $8}') MapToOutput $(xrandr | grep connected | grep -v dis | awk '{print $1}' | fzf --height 10% --reverse)
```

Use always the stylus' PID with:

```bash
$(xsetwacom list | grep "stylus" | awk '{print $8}')
```

Choose a connected display with this code:

> Note: using fzf as selector.

```bash
$(xrandr | grep connected | grep -v dis | awk '{print $1}' | fzf --height 10% --reverse)
```





## Change the default buttons behaviour

> Notes
> - stylus indexes start from 2 (the first one is when the pen touching the pad)
> - pad indexes stars from 1

```bash
$ xsetwacom set 28 Button 2 "key +Control_L z -Control_L"
$ # the smart version
$ xsetwacom set $(xsetwacom list | grep "stylus" | awk '{print $8}') Button 2 "key +Control_L z -Control_L"
```
