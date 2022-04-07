+++
author = "Abijith b"
title =  "Copy stuff from tmux in vi mode"
date = 2022-04-07T21:36:59+05:30
tags = [
    "tmux",
    "vim",
	"workflow"
]
+++

I have been using tmux for a while. I usually have two window, one for nvim
and another for running the server. Sometimes my server crash with weird
error message and copying that was a bit diffcult for me. It was embarrassing
if you are in call with a colleague. (I'm kinda hacker for my buddies as they
only use vscode :p)

So it was actually easy and you can do it with vi key bindings, just awesome.

## Edit your .tmux.conf

```sh
set-window-option -g mode-keys vi
bind -T copy-mode-vi v send-keys -X begin-selection
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'
```

## Entering Vi mode

press `Ctrl+b` then `[`

1. Ctrl + b is default prefix, change if you have overwritten default
2. navigate and copy with vi binds  🎉
