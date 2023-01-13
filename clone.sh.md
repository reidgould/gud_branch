---
title: clone.sh
layout: default
nav_order: 4
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Script File

`clone.sh` is located in Reid's script library.
If you have configured a bash environment (e.g. WSL or Git Bash) you can put the file in any folder listed in your `PATH` environment variable to make it convenient to run.

# Purpose

`clone.sh` performs several tasks that a normal clone does not.

* Checks that user and name are configured, and prompts if they are not.
* Uses the corresponding `.template` folder to correctly configure the user's local repository.
* Sets up to use the [Git Worktrees](./Git-Worktrees.html) feature.
  * Clones the user's local repository as a "bare" repository (the local `MyProject.git` folder).
  * Creates a worktree for the user to start with (the local `MyProject.worktree` folder). This worktree can be removed by the user, see instructions below.

# Usage

`clone.sh` must be executed in bash. To use bash on Windows, use WSL or Git Bash.

In bash, use `cd` to change directory to the location on your hard drive where you want a new folder containing your clone to be located.
Execute `clone.sh` by writing the full path to it's location on the network, and give it the required arguments. The first argument specifies the location of the remote repository. It can be a path to a repository on the shared drive.

Example:

```
clone.sh {{ site.github.repository_url }}
```

The optional arguments are useful for cloning existing projects.

The remote can be specified using any git supported protocol, like https.

The `--template` option specifies the template to use for local configuration. Use `--no-template` to force a clone without using a template.
The second argument, `myLocalClone`, is optional, it specifies the path of the folder that will be created for your clone.

Example:

```
clone.sh --template repo-windows.template {{site.github.repository_url}} myLocalClone
```

# Next

[Git Worktrees](./Git-Worktrees.html)
