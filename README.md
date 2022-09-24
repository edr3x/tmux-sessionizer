# tmux-sessionizer

tmux-sessionizer is a tmux script that makes your workflow BLAZINGLY FAST written by [ThePrimeagen](https://github.com/ThePrimeagen/)

## Installation

### tmux-sessionizer script

- Save the script on `~/.local/scripts/tmux-sessionizer` where `tmux-sessionizer` is the name of the script

```bash
#!/usr/bin/env bash

if [[ $# -eq 1 ]]; then
    selected=$1
else
    selected=$(find ~/projects ~/tests -mindepth 1 -maxdepth 1 -type d | fzf)
fi

if [[ -z $selected ]]; then
    exit 0
fi

selected_name=$(basename "$selected" | tr . _)
tmux_running=$(pgrep tmux)

if [[ -z $TMUX ]] && [[ -z $tmux_running ]]; then
    tmux new-session -s $selected_name -c $selected
    exit 0
fi

if ! tmux has-session -t=$selected_name 2> /dev/null; then
    tmux new-session -ds $selected_name -c $selected
fi

tmux switch-client -t $selected_name
```

- Here change the find paths on line no. 6 to your corresponding paths to projects folder on which you want to work on

## Add the script folder on your `.bashrc` or `.zshrc`

```bash
PATH="$PATH":"$HOME/.local/scripts/"
```

## Add the macro `ctrl+f` on your `.bashrc` or `.zshrc`

```bash
bindkey -s ^f "tmux-sessionizer\n"
```

## Add the following script on your `.tmux.conf`

```bash
bind-key -r f run-shell "tmux neww ~/.local/scripts/tmux-sessionizer"
```
- i haven't done other shortcuts, if you want you can see [here](https://github.com/ThePrimeagen/.dotfiles/blob/master/tmux/.tmux.conf), for the example on how it is done.

> This will create a new tmux session on your next desired project directory

## Finally run following command on your terminal to give permission for `tmux-sessionizer` script to run

```bash
chmod +x ~/.local/scripts/tmux-sessionizer
```

## Now restart your shell and enjoy the blazing fast workflow

### Steps to use

- Press `ctrl+f` to open the fzf finder
- type the name of project you want to work on and press enter
- Now you will be on the project directory on tmux session

> Note:
> 
> you can see prime's [video](https://youtu.be/hJzqEAf2U4I) on this
