---
title: UiPath git Filters - Greatly Reduce Merge Conflicts
layout: default
nav_order: 3
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# What is a git Filter? Why use StripViewState?

Git filters are related to the way git handles text files. When you commit a text file on Windows, git modifies the file from having `CRLF` line endings to have `LF` line endings, and converts them back to `CRLF` when they are checked out. This is required to use many of git's essential tools like diff and merge.

The filters on this page extend this behavior to modify `.xaml` files as specified in `.gitattributes` below. They remove view-only information that UiPath Studio saves in `.xaml` files which causes excessive merge conflicts. They do not restore the view information on checkout, but UiPath regenerates it when it opens the file again.

Filters are written in bash, which is a standard shell for Linux. They work on git for Windows because git ships with it's own "Git Bash" environment which it uses for repository hooks, filters, user commands, and more.

# Implementation

## Copy the Filters to Each User's PC

Copy the files below into one of the directories listed in the `PATH` variable of the bash environment used by git.

For "Git Bash", `/usr/bin` is a good location. For WSL, I prefer to use `/home/<user.name>/.local/bin`.

__Note:__ bash scripts _require_ Linux style line endings. When copying the script, double check in your text editor that the file is saved with `LF` only, not `CRLF`.

### The `StripViewState` file

```
#! /usr/bin/env bash
sed -E \
  -e '/<\/sap2010:WorkflowViewState.IdRef>/d' \
  -e 's/\s*sap2010:WorkflowViewState.IdRef="[^"]*"//g' \
  -e '/<\/sap:VirtualizedContainerService.HintSize>/d' \
  -e 's/\s*sap:VirtualizedContainerService.HintSize="[^"]*"//g' \
  -e '/<sap:WorkflowViewStateService.ViewState>/,/<\/sap:WorkflowViewStateService.ViewState>/!bx ; /x:Key="IsPinned"/!bx ; s/True/False/g ; :x ; ' \
  -e '/<sap:WorkflowViewStateService.ViewState>/,/<\/sap:WorkflowViewStateService.ViewState>/!bx ; /x:Key="ShouldExpandAll"/!bx ; s/True/False/g ; :x ; '
```

### The `StripBOM` file

```
#! /usr/bin/env bash
sed '1s/^\xEF\xBB\xBF//'
```

## Configure Each Repository

If you created your repository following the instructions in [Starting a UiPath Project](./Starting-a-UiPath-Project.html), these configurations are already included!

### Add to `.gitattributes`

Located in the work folder.

```
# UiPath filters.
*.xaml text filter=xaml diff=xaml
```

### Add to `config`

Located in the `.git` folder, not the work folder. You can also edit it using `git config -e`.

```
[filter "xaml"]
  clean = StripViewState | StripBOM
```

# Renormalize Existing Repositories

When adding these filters to an existing repository, you will need to create a commit to "renormalize" the committed files. It is easiest to merge any outstanding branches and do this once, otherwise you will have merge conflicts if you renormalize branches separately.

The `--update --renormalize` arguments to `add` tells git to run all currently tracked files through it's "clean" process which includes line ending conversion and filters. All files that were effected by the new filter should show up as being modified if you run `git status` after `add`. Commit the changes.

```
git add --update --renormalize
git commit -m 'Renormalize.'
```

# Next

[clone.sh](./clone.sh.html)
