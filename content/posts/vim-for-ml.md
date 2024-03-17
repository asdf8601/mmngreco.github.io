---
title: "Neovim for ML"
date: 2024-03-17
draft: false
categories: ["programming"]
labels: ["english", "machine-learning", "neovim"]
---

## Neovim for ML

I started my professional career with a role somewhat similar to DS, but as
time went on, I gravitated more towards a backend role. Partly because I
enjoyed it more and partly because it was an area I hadn't explored as much.

After several years of using PyCharm/VSCode, I switched to NeoVim. There was
quite the adaptation period, but now I don’t regret making the change at all.

Notebooks are really handy, but they're also chaotic, nonlinear and
comfortable. However, they are hard to maintain and take to production, not to
mention checking for changes.


## Neovim Plugins

- [jpalardy/vim-slime](https://github.com/jpalardy/vim-slime)
- [goerz/jupytext.vim](https://github.com/goerz/jupytext.vim)

## Dependencias python

- [IPython](https://ipython.org/)
- [plotly](https://plotly.com/python/)
- [pdbpp](https://github.com/pdbpp/pdbpp)


## Neovim Config


Here's my current Neovim configuration for running Python code in chunks, a bit
like a notebook, but in my opinion, it is more versatile.

> Note: I built my configuration from scratch (there could be errors I might
> have missed). I use LazyVim as a plugin manager.

```lua
  {
    'goerz/jupytext.vim',
    init=function ()
      -- will use `# %%` to define cells
      vim.g.jupytext_fmt = 'py:percent'
    end
  },
  {
    'jpalardy/vim-slime',
    init=function ()
      vim.g.slime_last_channel = { nil }
      -- will use `# %%` to define cells
      vim.g.slime_cell_delimiter = '\\s*#\\s*%%'
      vim.g.slime_paste_file = os.getenv("HOME") .. "/.slime_paste"

      local function next_cell()
        vim.fn.search(vim.g.slime_cell_delimiter)
      end

      local function prev_cell()
        vim.fn.search(vim.g.slime_cell_delimiter, "b")
      end

      vim.keymap.set('n', '<leader>e', vim.cmd.SlimeSend, { noremap = true, desc = 'send line to term' })
      vim.keymap.set('n', '<leader>cv', vim.cmd.SlimeConfig, { noremap = true, desc = "Open slime configuration" })
      vim.keymap.set('x', '<leader>e', '<Plug>SlimeRegionSend', { noremap = true, desc = 'send line to tmux' })
      vim.keymap.set('n', '<leader>ep', '<Plug>SlimeParagraphSend', { noremap = true, desc = "Send Paragraph with Slime" })
      vim.keymap.set('n', '<leader>ck', prev_cell, { noremap = true, desc = "Search backward for slime cell delimiter" })
      vim.keymap.set('n', '<leader>cj', next_cell, { noremap = true, desc = "Search forward for slime cell delimiter" })
      vim.keymap.set('n', '<leader>cc', '<Plug>SlimeSendCell', { noremap = true, desc = "Send cell to slime" })

      local slime_get_jobid = function()
        local buffers = vim.api.nvim_list_bufs()
        local terminal_buffers = { "Select terminal:\tjobid\tname", }
        local name = ""
        local jid = 1
        local chosen_terminal = 1

        for _, buf in ipairs(buffers) do
          if vim.bo[buf].buftype == 'terminal' then
            jid = vim.api.nvim_buf_get_var(buf, 'terminal_job_id')
            name = vim.api.nvim_buf_get_name(buf)
            table.insert(terminal_buffers, jid .. "\t" .. name)
          end
        end

        -- if there is more than one terminal, ask which one to use
        if #terminal_buffers > 2 then
          chosen_terminal = vim.fn.inputlist(terminal_buffers)
        else
          chosen_terminal = jid
        end

        if chosen_terminal then
          print("\n[slime] jobid chosen: ", chosen_terminal)
          return chosen_terminal
        else
          print("No terminal found")
        end
      end

      local function slime_use_tmux()
        vim.g.slime_target = "tmux"
        vim.g.slime_bracketed_paste = 1
        vim.g.slime_python_ipython = 0
        vim.g.slime_no_mappings = 1
        vim.g.slime_default_config = { socket_name = "default", target_pane = ":.2" }
        vim.g.slime_dont_ask_default = 1
      end

      local function slime_use_neovim()
        vim.g.slime_target = "neovim"
        vim.g.slime_bracketed_paste = 1
        vim.g.slime_python_ipython = 1
        vim.g.slime_no_mappings = 1
        vim.g.slime_get_jobid = slime_get_jobid
        -- vim.g.slime_default_config = nil
        -- vim.g.slime_dont_ask_default = 0
      end

      slime_use_neovim()
      -- slime_use_tmux()
      -- }}
    end
  },
```


## Workflow

I open neovim, where I usually split the window. I place my Python script in
one side, and on the other side, I open the terminal with `IPython`.

I use slime to send commands from the script window to `IPython`. One of the
biggest setbacks comes when viewing graphics. Matplotlib is pretty good, but
managing the qt or gke windows can be tiresome. For this reason, I've switched
to plotly, as it opens your graphics in the browser. This way, you have all
your graphs in one spot, and they are interactive too - it's a win-win.

On those rare occasions when I need to use someone else's notebook, I use
`jupytext`, which basically converts notebooks to other formats, including
Python scripts. When you open a notebook in neovim, this plugin will convert
the notebook into a Python script in the background, and when you save it, it
will convert it back into a notebook again - it works pretty well.

In the configuration above, I've included a set of settings in order to emulate
notebook cells using `# %%`, which allows me to include this comment and, when
executing `<leader>cc`, it will send all the content between these markers or
to the end of the file.

Honestly, I don't need more than this.

In fact, I get many more advantages than using VSCode or Notebooks, as I have
the full power of vim and my other plugins like ChatGPT, Copilot, CLIs, etc.

The good thing is that, in the end, you have a much easier to maintain Python
script.

`IPython` brings a wealth of useful features to the table. For example, it
enables the use of magic commands like `%run script.py`. But that's not all, it
also provides the capability to [autoload scripts or dependencies][ipyconfig],
making it a standout, dynamic tool. You can even [use rich][ipyrich], if you
like.

I hope you find this helpful. This same solution could potentially be used, but
instead of working on a Python script, you would do so on a markdown or quarto
file. It's all about trying out what works best for you.


{{< youtube 9PuF36UNOjM >}}


[ipyconfig]: https://ipython.readthedocs.io/en/stable/config/intro.html#example-configuration-file
[ipyrich]: https://rich.readthedocs.io/en/stable/introduction.html#ipython-extension
