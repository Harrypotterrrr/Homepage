---
layout: article
title: "Tmux tutorial"
sidebar:
  nav: Cookbook
aside:
  toc: true
tags:
  - cookbook
key:  blog-2019-06-20-Tmux-cookbook
date: 2019-06-20
modify_date: 2019-06-20
---

This is the tutorial of Tmux.
<!--more-->

## Introduction

Tmux is a nice and user-friendly software used on command line. It ensures the current task running even if the SSH is broken down abnormally, that is this software can replace the tradition `nohup` to some extent.

Briefly speaking, there are two features in this software.

1. split the windows into different and independent sub-terminal, and raise the efficiency.

2. reduce the risks of disconnection of SSH and increase security.

## Component

- session
- windows
- pane

## session management

- create new session

``` shell
tmux new [-s <session_name>]
```

- list session

``` shell
tmux ls
```

- connect to the last <the designated> session

``` shell
tmux a [-t <session_name>]
```

- close the session

``` shell
tmux kill-session [-t <session_name>]
tmux kill-server # close all
```

- list session in tmux: `prefix + s`

- rename the session: `prefix + $`

- detach from the present session: `prefix + d`

## windows management

- create a new windows: `prefix + c`

- rename the windows: `prefix + ,`

- list windows: `prefix + w`

- next/previous/last windows: `prefix + n/p/l`

- choose windows: `prefix + 0~9`

- close windows: `prefix + &`

## pane management

- create horizonal pane: `prefix + %`

- create vertical pane: `prefix + "`

- switch pane: `prefix + up/down/left/right`

- display the pane number: `prefix + q`

- close pane: `prefix + x`

- rearrange pane: `prefix + space`

- show time: `prefix + t`

- zone in the pane: `prefix + z` 

## Configuration

- create a tmux config file

``` shell
vim ~/.tmux.conf
```

- reconfigure the prefix and mouse mode

append the following content to `~/.tmux.conf`

``` shell
# remap prefix from 'C-b' to 'C-a'
set-option -g prefix C-x
unbind-key C-b
bind-key C-x send-prefix

# Use Alt-arrow keys to switch panes
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# Mouse mode
setw -g mode-mouse on
```

- restart the tmux

``` shell
tmux source-file ~/.tmux.conf
```
