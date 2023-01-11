---
title: Use Notepad++ as Your Text Editor in Git Bash
layout: default
nav_order: 6
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Config

Edit the global config file. You can using the following command to open it in the currently configured editor, which may be a CLI editor, but you'll only have to do it once! 
`git config -e --global`

Paste in this configuration. If you already have a `[core]` section, the line can go inside of it.

```
[core]
  editor = 'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin
```

# How It Works

## Editing a git Text Editor Prompt

Any git command that asks for user input will open a Notepad++ window.
For example, `git commit` to write a message, `git config -e` to edit the local repo config file, or `git rebase -i` to edit the action list.

Git will stop and wait for the editor process to end before it continues.

Read the instructions in the window (usually commented by `#` symbols). Make edits as needed. Save the file (you can save multiple times while editing if needed). Close the Notepad++ window.

## Canceling an Action From The Text Editor

Sometimes it is useful to stop an action after git has already opened an editor. For example, if your use `git rebase -i` and when the editor opens, the action list is not what you expect, and you realize you have given an incorrect "onto" or "upstream" reference.

Click on the "Git Bash" window where git is waiting for the editor. Press "Control-C" (a commonly used signal in Linux shells to kill a running command). Close the editor. It doesn't matter if you have saved the file or not.

Git will print a message containing something like `error: There was a problem with the editor ...` and the command will not proceed.

# Troubleshooting

Make sure there is not a local configuration overriding the global one.

Check in the local config file for the repository you are using:
`git config -e`

Check in "Git Bash" that the `GIT_EDITOR` environment variable is not set. 
The command `echo $GIT_EDITOR` should have no output.
If the variable is set, look in `~/.bashrc` and `~/.bash_profile` and remove the configuration if found.
If the configruation cannot be found, add a line at the end of `~/.bashrc` like the following:

```
unset GIT_EDITOR
```

# Next

[Branching Strategies](./Branching-Strategies/Branching-Strategies.html)
