---
title: "Takt: A Simple and User-Friendly Time Tracker"
date: 2023-08-27
draft: false
categories: ["programming"]
labels: ["hacking", "time-tracker", "cli", "takt", "hacking", "time", "terminal", "productivity", "time-management"]
---

## Context

The concept behind [Takt][takt] is to track time in a simple and human-readable
manner that is easy to edit.

I have used time management tools like Factorial and Kenjo, and while they have
beautiful front-end interfaces, they often neglect the back-end. We, on the
other hand, prefer to do everything from the console.

Wouldn't it be cool to be able to execute `curl
.https://awesome-tm/check?time=now` and automatically have a check in/out as
appropriate? Or to be able to run `curl https://awesome-tm/summary` and see a
summary of your worked time?

This seems basic, simple, and anyone can use it. If you get up for a coffee and
don't want to lose track of time, you can simply launch a `tm-check` from the
console. You can even have a shortcut that does this!

> Idea: `alias tm-check='curl .https://awesome-tm/check?time=now'`

If anyone knows a reason why this can't be the case, I would love to hear it.

## Takt

With this in mind, I began to write something that does exactly that, but
locally. I named it [`takt`][takt] (derived from the German word Taktzeit,
meaning cycle time).

At the moment, it's very simple but does exactly what it's supposed to. Over
time, I would like to have integrations with other applications and be able to
do `pull`/`push` as if it were a git repository. We'll see how it evolves.

![screenshot](https://media.hachyderm.io/media_attachments/files/110/963/065/168/213/242/original/b431a99f64c70415.png)


<!-- Links -->
[takt]: https://github.com/mmngreco/takt
