---
title: "Using i3 WM on MacOS with Yabai and Skhd"
date: 2023-08-25
draft: false
categories: ["hacking"]
labels: ["i3", "macos", "yabai", "wm", "window-manager"]
---

## Transitioning from Linux to MacOS

Having used Ubuntu/Pop_OS for several years both personally and professionally,
I've become well-versed in the Linux ecosystem. I have numerous scripts and a
comprehensive list of programs, along with my `dotfiles` for easy
configuration.

However, upon switching companies, I was required to use a Mac. This transition
was challenging, especially since I missed the efficiency of a good window
manager. On Linux, I used i3, which significantly boosted my productivity.

## Implementing i3 on MacOS with Yabai and Skhd

After researching "i3 on macOS", I discovered several alternatives, with
[`yabai`][yabai] emerging as the best option. It, along with [`skhd`][skhd],
effectively replicates the i3 experience on MacOS.

## Installation of `yabai` and `skhd`

```bash
# install
brew install koekeishiya/formulae/skhd
brew install koekeishiya/formulae/yabai

# start service
skhd --start-service
yabai --start-service

# if you change your yabairc
yabai --restart-service
```

To ensure everything works correctly, you need to grant access to `skhd` and
`yabai` on MacOS. After a restart, everything should function as expected.


## Useful Links

- [yabai's wiki][wiki]
- [How To Use A Tiling Window Manager On MacOs | Yabai Ultimate Guide][yt]
- [using-i3-like-tiling-window-managers-in-macos-with-yabai][medium]
- [`yabairc`][yabairc]


<!-- Links -->

[yt]:https://www.youtube.com/watch?v=k94qImbFKWE
[skhd]: https://github.com/koekeishiya/skhd
[yabai]: https://github.com/koekeishiya/yabai
[wiki]: https://github.com/koekeishiya/yabai/wiki
[yabairc]: https://github.com/mmngreco/dotfiles/blob/master/macos/home/.config/yabai/yabairc
[medium]: https://anuj-chandra.medium.com/using-i3-like-tiling-window-managers-in-macos-with-yabai-ebf0e002b992
