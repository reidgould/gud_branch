---
title: Reid's git Tips and Scripts
layout: default
nav_order: 8
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Tips

## Read The Docs!

The [Pro Git book](http://git-scm.com/book/en/v2){:target="_blank"} is free on the official website in html and is really good!

The [Reference](https://git-scm.com/docs){:target="_blank"} is very descriptive with examples of each command. The latest is online, but the copy for your exact version also comes built in! On "Git Bash", use the command `git help` to open the local html copy in your browser for any command like `git help commit`. If you're using a real Linux system like WSL, use `man git` or `man git-commit`.

## Git is Not A Desktop Program

Don't think of git like a desktop program, think of it like a collection of scripts to move plain old files in useful patterns.

## Git Takes Work

Git is a tool. Like a hammer, it takes work to use. It’s up you to decide if it delivers enough value to be worth it.

## Branches are Simple Pointers

Branches are simple pointers to a single commit, not like folders of commits. Git doesn't remember which branch a commit was made on, instead it is said that a commit is "reachable" from a branch by following the a commit's pointers to its parents. After merging, a commits are often "reachable" from multiple branches. The file in the `.git` folder that "stores" a branch only contains the hash of the commit it points to. That's it!

More info in the [Core Concepts section - Refs and Objects: HEAD, Branch, Commit, Tree, and Blob](./Core-Concepts.html#refs-and-objects-head-branch-commit-tree-and-blob)

## Commit Small Changes

Don't hesitate to fix a typo or small bug in it's own commit. As seen in the [Core Concepts section - Three Layers of Abstraction](./Core-Concepts.html#three-layers-of-abstraction), git is efficient at dealing with the data, developers should take advantage of the abstractions to work effectively. 

When doing a complicated rebase or difficult merge, multiple clearly separated commits are much easier to think about and resolve each item one at a time rather than having to think about all the differences at once.

## Immutability Gives You Safety But Don't Make These Mistakes

In the [Core Concepts section - Immutability Gives You Safety to Make Changes](./Core-Concepts.html#immutability-gives-you-safety-to-make-changes) there's an example of how to use `reflog` to recover unreachable commits. However, there are a few cases that immutability can't protect you from.

__Risks and Mitigations:__
* The Working Directory is not in the repository. Some commands like `git reset --hard` may permanently delete uncommitted files. Use `git stash` to avoid this.
* Unreferenced commits eventually get deleted by garbage collection. Tag any commit to keep it forever.
* Any change made to the repository `.git` folder without using a git command has potential to corrupt your repository. Use [Git Worktrees](./Git-Worktrees.html) so that the `.git` folder is not in the default location inside your Working Directory where it may be accidentally modified.
* Your computer can crash! Commit and push work in progress to the central repository at least once a day. [Contributor Branch Namespaces](./Branching-Strategies/Contributor-Branch-Namespaces.html) are a good way to keep different developers' work separated, but still backed up, until it's time to integrate.

## Don't Copy Project Folders - Branch

Don't make a copy of project code

Start a feature or wip branch, and use a [worktree](./Git-Worktrees.html) to `checkout` multiple branches at once.

or a separate tree history started as an orphan branch from `root`.

## Don't Comment Out Code - Tag

If you want to keep some code that shouldn't run right now, don't comment it out. Instead, delete it and tag the old commit. Include a message with the tag that describes the code section you want to remember.

Commented out code quickly becomes stale. The program around it changes, and it wouldn't even work if you enabled it again. It stops making sense in the context, so it isn't even a good reference any more.

If you want to comment a section to write a different version, simply make the changes in a new commit and use `git diff` to compare the two versions.

# Scripts

[clone.sh](./clone.sh.html) is the only one I will directly ask you to use.
I’m happy to share the others, just ask if you are interested.

## Conveniently Update the gitbranch File

* `gbranchpush`
* `gbranchpop`
* `gbranchpeek`

## Log Shortcuts

* `glog`, `glogc`, `glogn`, `glogf`
* `glo`, `glogn`
* `glogstash`

## Check Work Status

* `gckcon`
  "git check conflicts" Prints the names of files that have changes on both commits given compared to their common ancestor.
* `gcku`
  "git check upstream" Show branches and their upstreams and tracking status. Print branches without upstreams.
* `gckr`
  "git remote check" Check that a remote has refs (especially useful with pattern "refs/tags") that the local repo has.

## List Refs for Filtering

* `gfer`
  Formats for-each-ref as space separated words without common prefixes for input to commands that expect symbolic references.
* `gsr`
  Formats show-ref as space separated words without common prefixes for input to commands that expect symbolic references.
* `wordrm`
  Word Remove. Removes words given before the first "--" argument from the set of words given after, and prints them to stdout delimited by spaces.

## Get Files from Other Branches

* `gcoc`
  "git checkout commit" Checkout the file at the given commit int the worktree with a different name.
* `gcol`
  "git checkout last" Checkout the version of a file at HEAD into the worktree with a different name.
* `gcoot`
  "git checkout ours and theirs" During a conflict, check out full versions of both files into the worktree with different names.

## Subtree Strategies

Recommended by article [Mastering Git Subtrees](https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec){:target="_blank"} instead of using git Modules.

* `gsread`
* `gsmerge`
* `gspick`
* `gsfilter`

## Execute a Command Multiple Times

* `gxg`
  "git execute .git" Run a command in each ".git" repository folder in the current working directory. Works well for repositories cloned with the `--separate-git-dir` argument like in `clone.sh`.
* `gxw`
  "git execute worktrees" Run a command in each worktree in the current repository.
* `gxr`
  "git execute remotes" Run a command on each remote in the current repository.
* `gxer`
  "git execute each ref" Run a command on each ref determined by your filter.
* `gxb`
  "git execute branches" Run a command on each branch determined by your filter.
* `gxt`
  "git execute tags" Run a command on each tag determined by your filter.

## Garbage Collection
* `ggcnow` and `ggcnowaggressive`
  Force cleanup of unused objects. Reduce repository size.
