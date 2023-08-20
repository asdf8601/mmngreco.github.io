---
title: "Have you heard of rich?"
date: 2023-08-20
draft: false
categories: ["hacking"]
labels: ["rich", "hack", "python", "console", "terminal"]
---

## Have you heard of rich?

Honestly, I don't remember how long I've been following [Will's][Will] work,
but I find it very interesting and educational. Among Will's work are
[`rich`][rich] and [`textual`][textual], both projects designed to extend the
use of the terminal.

One thing you can see in Will's work is the modular design. On one hand,
there's `rich`, which basically gives you the necessary tools to format console
output to your liking, making the console output clearer and more attractive.

Then we have [`textual`][textual], a framework for making TUIs (terminal user
interfaces) that depends on [`rich`][rich]. The value of `textual` is not only
being able to create TUIs in Python easily and elegantly, but also that the TUI
you develop could run in a web browser! The same application can run in the
terminal or in the browser. This is music to the ears of many backends.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Textual apps running in a browser!<br><br>I intentionally obscured the address bar, as I&#39;m not quite ready to share the dev URL. <a href="https://t.co/7ztn7r8G0o">pic.twitter.com/7ztn7r8G0o</a></p>&mdash; Will McGugan (@willmcgugan) <a href="https://twitter.com/willmcgugan/status/1690386937264119808?ref_src=twsrc%5Etfw">August 12, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The most immediate use I can think of is that I could write many internal
applications easily that I can share with the team, regardless of the hacking
skills of my team members.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The Textual web service signup dialog is looking good. Now to implement the backend. <a href="https://t.co/2z2wqRQi54">pic.twitter.com/2z2wqRQi54</a></p>&mdash; Will McGugan (@willmcgugan) <a href="https://twitter.com/willmcgugan/status/1691396483524820993?ref_src=twsrc%5Etfw">August 15, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


## What do you use rich for?

`rich` has a series of ready-to-use functions that you can take advantage of
without having to build anything additionally.


### Better print

> https://rich.readthedocs.io/en/stable/pretty.html

`rich` has made me love using `print` again. I no longer use the built-in
`print`, but use `rich`'s:

```python
from rich import print
```

For example, `print(my_json)` with rich, the output is much more elegant and
readable than using `pprint`. Other examples are `print(my_list)`,
`print(my_dict)`, etc.


### Better Logging

> https://rich.readthedocs.io/en/stable/logging.html

```python
import logging
from rich.logging import RichHandler

FORMAT = "%(message)s"
logging.basicConfig(
    level="NOTSET", format=FORMAT, datefmt="[%X]", handlers=[RichHandler()]
)

log = logging.getLogger("rich")
log.info("Hello, World!")
```


### Better Traceback

> https://rich.readthedocs.io/en/stable/traceback.html
```python
from rich.traceback import install
install(show_locals=True)
```


### DIY

Then you have a series of objects with which you can build really useful and
beautiful console outputs, such as `columns`, `tables`, `layouts`, etc.

In the [documentation][rich-doc], you have everything you need to get started.


Just try it, you will enjoy the output.

<!-- Links -->

[Will]: https://www.willmcgugan.com/
[rich]: https://github.com/Textualize/rich
[textual]: https://www.textualize.io/projects/#textual
[rich-doc]: https://rich.readthedocs.io/en/stable/pretty.html
