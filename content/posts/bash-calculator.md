---
title: "I Wish I Had a Simple Calculator in My Bash"
date: 2024-11-25T20:26:21+01:00
draft: false
categories: ["programming"]
tags: ["bash", "functions"]
---


I'm sure you've found yourself needing to do quick calculations in the
terminal. Sure, you could use a calculator app or a fancy spotlight bar, but
sometimes the calculation is so simple that typing it directly would be easier.
Wouldn't it be nice if you could just type `math "200 ** 2 / 200"` and get your
answer right away?


## Context

Here's a "real" example. Let's say you're checking some logs:
```
INFO combinations trained: 245
INFO documents written: 71
```

and you want to figure out roughly how many combinations each document has - in
other words, you need to calculate `245 / 71`.

## Option B2

Probably the most general (and awesome) solution would be to write a
super-duper Bash function that can be summoned at any time in your terminal,
like a magical math genie:

```bash
# put this in your bashrc / zshrc
calcpy {
  python -c "from math import *; print($@)"
}
```

Run this in your terminal—because why not?

```
$ calcpy "245 / 71"
3.4507042253521125
```

## Option C1

If you're in Neovim (like 90% of the time), you can conjure a Vim command for
this:

```vim
command! -nargs=+ Calc :execute '!python -c "from math import *; print(<args>)"'
```

Or use the Lua interface, because who doesn't love a bit of Lua magic in their
life? 🌙

```lua
vim.api.nvim_create_user_command('Calc', function(opts)
  vim.cmd('!python -c "from math import *; print(' .. opts.args .. ')"')
end, { nargs = '+' })
```

In both cases, you can summon your mathematical powers like this:

```vim
:Calc 245 / 71
3.4027777777777777
```
