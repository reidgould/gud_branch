---
title: The gitbranch File - Start & End Branch Commits
layout: default
nav_order: 1
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# What gitbranch Contains

The `.gitbranch` file is not standard in git, it is a file you can use to record what branch a commit was made to.

`.gitbranch` contains one branch name per line to form a stack indicating the hierarchy that the current branch is based on and therefore where the work will eventually be merged. The following is an example of what you may find in `.gitbranch`.

```
feature/newFeature
semver/minor
semver/major
```

# What gitbranch is Good For

## Clarity in the Work Tree

Branches in git are [simply pointers](../../Core-Concepts.html#refs-and-objects-head-branch-commit-tree-and-blob).

There are several cases it may not be clear what branch files you are viewing were actually committed to. This can be because the files are being viewed outside of the repository or because HEAD is "detached" for various reasons.

* Viewing an exported a archive.
* Resolving conflicts during a rebase (especially one with `--rebase-merges`).
* Using `git bisect`.
* In a script for `git filter-branch`.

Use of the `.gitbranch` file records branch information in the commit to help clarify where the commit was made.

## Readability of the Log Graph

Historical branches should be removed.

The `.gitbranch` file is useful for creating start and end commits.
These make branches distinct in log view and are useful as handles when doing repository operations like rebases.

The following log graph has "start branch" and "end branch" commits marked with `»`. Since all the branches have been merged, they have also been removed. The branch names are still clear because of the messages on the start and end commits created with `.gitbranch`.

```
  * 77534d7 (top) top
  *   676aac5 Merge branch semver/major.
  |\  
» | * 68ea9fe End branch "semver/major".
  | * 6196a9e (stage/production, tag: semver/1.0.0) 1.0.0
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
» | | * a115621 End branch "semver/minor".
  | | *   5ff7c11 Merge branch "semver/patch".
  | | |\  
» | | | * 3dc530a End branch "semver/patch".
  | | | * 51bba9e (tag: semver/0.1.1) 0.1.1
  | | | * 1a3b91e Changes
  | | | * 1a3b91e Changes
» | | | * f07cf19 Start branch "semver/patch".
  | | |/  
  | | * c955d70 (tag: semver/0.1.0) 0.1.0
  | | * 4a5131f Changes
  | | * 46d3bff Changes
  | | * 77d858d Change "name" and "description" in "project.json".
» | | * a421cd8 Start branch "semver/minor".
  | |/  
» | * a82ec76 Start branch "semver/major".
  |/  
  * b70ccd4 Non merge branch parent.
  *   62c16cf Merge branch "feat/repoSetup".
  |\  
» | * a38e260 End branch "feat/repoSetup".
  | * d20a5c1 Create blank project and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
» | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

# How To Use gitbranch

Each time you start a branch, edit `.gitbranch` add the new branch name to the top of the file.
`git checkout -b new/branch`
`vim .gitbranch` (Use your preferred text editor to add the name.)
`git add .gitbranch`
`git commit -m 'Start branch "new/branch"'`

Each time you end a branch to merge it, remove it's name from the top.
`vim .gitbranch` (Use your preferred text editor to remove the name.)
`git add .gitbranch`
`git commit -m 'End branch "new/branch"'`
`git checkout base/branch`
`git merge --no-ff new/branch -m 'Merge branch "new/branch"'`

I've [written some bash functions](../../Reid's-git-Tips-and-Scripts.html#conveniently-update-the-gitbranch-file) to update `.gitbranch` with convenient one liners. Feel free to ask me about them.

# Next

[Mainline Commits Must Pass Tests](./Mainline-Commits-Must-Pass-Tests.html)