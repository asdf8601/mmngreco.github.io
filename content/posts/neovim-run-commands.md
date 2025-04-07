---
title: "Vim Tips: Running Commands"
date: 2024-11-14T18:21:57+01:00
draft: false
categories: ["vim"]
labels: ["english", "neovim", "tips"]
---


Let's explore a few different ways to run commands in our terminal from
the comfort of neovim. Trust me, it's cooler than it sounds! 😎

Here's a list of commands that I use all the time (and you should too!).

```bash{linenos=false}
# run a command
:!sh <cmd>

# execute the line and replace with the output
:.!sh

# execute without replacing
:.w !sh

# execute without replacing selection and it will persist until next selection!
:'<,'>w !sh

# execute a the line 8
:8w !sh

# execute and replace a the line 8
:8 !sh

# execute the selection in python
:'<,'>w !python -c "$(cat)"

# execute and replace the selection in python
:'<,'>!python -c "$(cat)"

```

## Hands-on

Hey! This is my absolute favorite command ever! I use it all the time when I'm
writing docs or README files because, let's face it - who wants to manually
copy-paste code outputs? Not me! 😅 Instead of doing things the boring way, I
use this neat little trick to get things done quickly. Plus, it's super handy
when you want to format text using other tools too like sql queries!


```vim{linenos=false}
     1 ls
    >2 ls
     ~
     ~
     ~
     NORMAL file.md [+]
:.!sh
```

And check out what happens:

```vim{linenos=false}
     1 ls
     2 Makefile
     3 README.md
     4 archetypes
     5 config.toml
     NORMAL file.md [+]
```

See what happened there? Instead of switching back and forth between your
terminal and Vim (so annoying, right?), we just ran the `ls` command right from
Vim! Pretty neat, huh?

When we use `:.!sh`, Vim runs the current line as a shell command and shows us
what happened. It's like having a mini-terminal inside your editor - how cool
is that?

This is super handy when you're writing docs or just need to quickly check
something without leaving Vim. No more alt-tabbing between windows like a
maniac!

Think of it as your editor being your personal assistant - "Hey Vim, could you
run this command for me?" And boom! There's your output, right where you need
it.

Simple, right? And trust me, once you start using this, you'll wonder how you
ever lived without it! 🎯


### Run a command without replacing the buffer

Let's see another example where I want to peek at the output without messing up
what's already in my buffer (because hey, sometimes we just want to look, not
touch! 😉)

```vim{linenos=false}
     1 ls
     ~
     ~
     ~
     ~
     NORMAL file.md [+]
:.w !sh
Makefile
README.md
archetypes
config.toml
content
data
layouts
public
resources
static
themes

Press ENTER or type command to continue
```

So what's happening here? Well, instead of replacing our precious `ls` command
with its output (like we did before), we're just taking a quick look at what's
in our directory. It's like opening your fridge to check what's inside without
actually grabbing anything! 🚀

The command `:.w !sh` is basically telling Vim: "Hey buddy, run this line for
me, but don't mess with my text, just show me what happens!" And Vim's like
"Sure thing, here's your file list!"

After it shows you all your files and folders, it politely waits for you to
press ENTER before going back to business. How nice is that? 😊

## Now, Think out of the box


Hey, imagine you're writing a Python function (like the one below) and you get
stuck - maybe you forgot a module name (we've all been there! 🤪). Instead of
doing the whole open-new-terminal-and-copy-paste dance, you can use this cool
`:w !python -c "$(cat)"` trick:

1. grab those first 6 lines of code below.
2. type `:w !python -c "$(cat)"` (I know, it looks weird, but trust me!)
3. Boom! You're done! 🎉

```python
def rand_exp() -> str:
  import random
  x = random.random()
  return x * x

print(sum(map(lambda x: rand_exp(), range(100))))
```

Here's another fun one:

```python
import json
res = [{'a': 1, 'b': 2, 'c': [1,2,3]}]
print(json.dumps(res))
```


## Summary

We saw a bunch of cool ways to run commands from neovim, and I bet you can come
up with even more awesome tricks than what we covered here! Go ahead and play
around with neovim's superpowers - before you know it, you'll be zipping
through your code like a Jedi Master! 🚀✨
