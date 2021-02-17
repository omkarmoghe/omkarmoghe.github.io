---
layout: project
title: "Dotfiles"
github_url: "https://github.com/omkarmoghe/dotfiles"
languages:
  - ZSH
emoji: ⚙️
order: 5
---

Not really a project, but finally got around to adding my [dotfiles](https://wiki.debian.org/DotFiles) to some form of VCS. Excited to not have to email/Slack these to myself from different machines like a caveman.

The `Makefile` provides recipes to sym link the ZSH config and GitHub user config to the user's `$HOME` folder (i.e. `~`). The `.zshrc` automatically loads other `.zsh` files from the dotfiles repo to populate the env, set up aliases, and anything else. Random learnings from building these scripts:
- Sharing variables across Make targets is annoying. I initially wanted to set some vars for all targets to avoid calling `$(PWD)` a bunch.
- Trying to get the target file dir from a sym link is annoying. I wanted to avoid forcing the user to set the `$DOTFILE_ROOT` in the `.zshrc` for auto-loading.

I run Ubuntu on a custom built PC (via [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-index)) as well as OSX (MacOS?) on a MacBook Pro so the goal was to make everything as OS agnostic as possible.

The Makefile and auto-loading scripts were heavily inspired by [Lars Kappert's post](https://medium.com/@webprolific/getting-started-with-dotfiles-43c3602fd789) as well as [Zach Holman's dotfiles](https://github.com/holman/dotfiles). I think those are great places to start.
