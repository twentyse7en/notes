+++
author = "Abijith B"
title = "Stow dotfiles"
date = 2022-04-03T02:12:32+05:30
tags = [
    "unix",
    "work",
    "productivity"
]
+++

## What is dotfiles
Basically configuration files. For example nvim comes with default configuration say 8 spaces for tabs, but mostly people configure it to 4 spaces by editing config file of nvim. Config file will be located in
`~/.config/nvim/init.vim`

## Why manage dotfiles?
I have two systems, one for work and other for personal use. I would
like to sync my nvim config in both. It would be good to backup
and you don't lose your precious config made of so many tweaks.

## How to manage
Maintain a directory so you can sync with github. 
1. So there is already `.config` directory is there a need to another one?<br/>
It may contain other config files you dont' need to maintain, some of them may be os specific. keep a `.dotfiles/` directory and move your configuration
to there.

2. But apps will be looking for config in `.config/`, right? <br/>
Yes, So we need a copy of that config in `.config/`. It is will be better
to create symblinks which will be just shortcuts.

## GNU Stow | symlink farm manager

Simply type `stow *` from your `~/.dotfiles/` all your symlink will be inside
your config directory and you can live happily ever after.

I'm not joking it's that simple, but you need to keep a structure. You need to keep a convention of creating directories, If you need to move
`nvim/init.vim` from your `.config/` to `.dotfiles/`,
1. You need to create following dir structure `.dotfiles/nvim/.config`
2. move files to that directory, now the directory will be `.dotfiles/nvim/.config/nvim/init.vim`
3. execute `stow *` inside `~/.dotfiles/`
4. Now you can see symlink in `.config` directory

everything work fine. Now backup your `.dotfiles/` directory

## Why this structure?
Read if you are interested to know why you need to keep this directory structure.

Stow is originally designed to manage symlinks when installing files locally. Will
go through example. When you build a package locally, say emacs it will contain
following structure.

```sh
emacs
    \__ bin/emacs
    \__ man/emacs.1
```

But it should be placed inside `usr/local` to get access. This will be target directory.

```sh
/usr/local/
        \___bin/emacs
        \___man/emacs.1
```

For this, the package should be kept in a `Stow directory`, `/usr/local/stow/`
all package will be placed inside there.

```sh
/usr/local/stow
            \___ emacs
            \___ nvim
```

when you `stow` files in `stow directory`, the emacs/bin files will go local/bin and soon.

## Reference
1. [Gnu Stow](https://www.gnu.org/software/stow/manual/stow.html)
