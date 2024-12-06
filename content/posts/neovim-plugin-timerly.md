---
title: "Neovim Pluging: Timerly (pomodoro)"
date: 2024-12-06T09:16:33+01:00
draft: true
---


Pomodoro float window in neovim

- https://github.com/nvzone/timerly


```lua
  {
    -- clock in neovim (pomodoro)
    -- https://github.com/nvzone/timerly
    "nvzone/timerly",
    lazy=false,
    dependencies = {
      'nvzone/volt'
    },
    cmd = "TimerlyToggle",
    config = function()
        require('timerly').setup()
        vim.keymap.set('n', '<leader>tc', ':TimerlyToggle<CR>', { desc = "Toggle Timerly" })
    end,
  },
```




