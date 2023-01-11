---
title: Git Worktrees
layout: default
nav_order: 5
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Official documentation

https://git-scm.com/docs/git-worktree

# Summary

Worktrees are beneficial in a git workflow for several reasons:

* The `.git` folder is outside of the folder where the user works (A default clone has the repository objects in the ".git" folder, which is inside the work folder).
In a separate worktree, there is less chance to accidentally modify or delete something in ".git", which usually causes the repository to be corrupted and unrecoverable.
* With worktrees, the user can have multiple branches checked out at once (A default clone has only one branch checked out in the only work folder).
See [Branching Strategies](./Branching-Strategies/Branching-Strategies.html) for examples of how this enables advanced workflows.

# Examples

## List Worktrees

`git worktree list`

## Add a Worktree

1. Navigate to the repository.
`cd MyProject.git`
1. Create a "work in progress" branch.
`git branch wip semver/minor`
1. Add a worktree.
`git worktree add ../MyProject.wip wip`
1. Navigate to the worktree.
`cd ../MyProject.wip`

## Remove a Worktree 

__Note:__ When you remove a worktree, untracked changes files (any created there that are not committed) are deleted without opportunity for recovery.

1. Confirm that there are no uncommited changes.
`cd MyProject.wip`
`git status`
1. Navigate to the reposiory.
`cd ../MyProject.git`
1. Remove the worktree.
`git worktree remove ../MyProject.wip`

# Next

[Use Notepad++ as Your Text Editor in Git Bash](./Use-Notepad++-as-Your-Text-Editor-in-Git-Bash.html)
