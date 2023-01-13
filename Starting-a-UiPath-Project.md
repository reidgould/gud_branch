---
title: Starting a UiPath Project
layout: default
nav_order: 2
nav_exclude: true
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Resources

 Files
* Script that works like an enhanced `git clone`.
  * [`clone.sh`](./clone.sh.html)
* For a new process, pass these as arguments to `clone.sh`.
  * `repo-uipath-process.git`
  * `repo-uipath-process.template`
* For a new library, pass these as arguments to `clone.sh`.
  * `repo-uipath-library.git`
  * `repo-uipath-library.template`
* For an empty remote in a shared drive, copy and rename this folder.
  * `repo-remote.git`

# Starter Projects

The `repo-uipath-process` and `repo-uipath-library` projects have several tasks completed to simplify creating a new project:

* Repository already set up and configured. Including `.gitignore`, `.gitattributes`, eol settings, and [filters to reduce merge conflicts](./UiPath-git-Filters-Greatly-Reduce-Merge-Conflicts.html).
* Core UiPath dependencies standardized (updated each time we upgrade Studio) and set to "static". This ensures use of consistent versions across projects and prevents conflicts with common libraries like LogJSON.
* LogJSON dependency installed.

# Instructions

## 1. Clone a Starter Project

Instead of using a normal `git clone`, please use [`clone.sh`](./clone.sh.html). There are several benefits, one of which is being set up to use [Git Worktrees](./Git-Worktrees.html).

## 2. Initialize the New Project

Run `initRepo.bash`. This removes the template from the repository's remotes and reauthors the existing commits so they don't collide with the template's commits.

## 3. Add the Azure DevOps Remote and Push

```
git remote add origin url_to_azue_devops_repo
git push origin --all --set-upstream
```

# Next

[UiPath git Filters - Greatly Reduce Merge Conflicts](./UiPath-git-Filters-Greatly-Reduce-Merge-Conflicts.html)
