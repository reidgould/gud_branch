---
title: Mainline with Semantic Versioning
layout: default
nav_order: 1
parent: Branching Strategies
has_children: true
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Versions and Stages

"Mainline" is the most simple strategy of having only a single branch with all the repository's history.  It might look like the following log graph.

```
* ae6eba3 (master) Release version 1.
* b938da1 Work on project.
* ca66bb7 Create repository.
```

However, UiPath Processes and Libraries are published with [“semver” (Semantic Versioning)](https://semver.org){:target="_blank"} numbers consisting of a major, minor, and patch component. Usage of a three tier mainline strategy aligns well with semver, providing a clear way to publish patches to the current production version while working on features. 

* Use branches `semver/major`, `semver/minor`, and `semver/patch` instead of a `master` branch. 
* Make a commit for each version bump and _tag_ it with the version number like `semver/1.0.0`. 
* Use branches like `stage/development`, `stage/qa`, `stage/beta`, and `stage/production` to indicate what version is deployed and for pipeline triggers. 

In the following example log graph, notice how the repository starts with a single commit, where the `root` branch is, and ends with a single commit where the `top` branch is. This demonstrates the commitment of all branches to reintegrate to the mainline.
The graph also has a branch at each of the three semver levels, which is seen in the way the `*` symbols for commit on those branches is shifted to the right, and connected by the bar and slash symbols. (Not pictured: a `semver/patch` branch can be based on a `semver/major` branch without a `semver/minor` between, the same way you can publish `1.0.1` before publishing `1.1.0`.)
The graph shows a repository with minor and patch releases that occurred during initial development, and a single major release when the project reached version `1.0.0`, which is currently in the production stage. There is no work in progress.

```
* 77534d7 (top) top
*   676aac5 Merge branch semver/major.
|\  
| * 68ea9fe End branch "semver/major".
| * 6196a9e (stage/production, semver/major, tag: semver/1.0.0) 1.0.0
| *   5ff7c11 Merge branch "semver/minor".
| |\  
| | * a115621 End branch "semver/minor".
| | *   5ff7c11 Merge branch "semver/patch".
| | |\  
| | | * 3dc530a End branch "semver/patch".
| | | * 51bba9e (semver/patch, tag: semver/0.1.1) 0.1.1
| | | * 1a3b91e Changes
| | | * 1a3b91e Changes
| | | * f07cf19 Start branch "semver/patch".
| | |/  
| | * c955d70 (semver/minor, tag: semver/0.1.0) 0.1.0
| | * 4a5131f Changes
| | * 46d3bff Changes
| | * 77d858d Change "name" and "description" in "project.json".
| | * a421cd8 Start branch "semver/minor".
| |/  
| * a82ec76 Start branch "semver/major".
|/  
* b70ccd4 Non merge branch parent.
*   62c16cf Merge branch "feat/repoSetup".
|\  
| * a38e260 End branch "feat/repoSetup".
| * d20a5c1 Create blank UiPath Process and add standard dependencies.
| * fd45a8f Create ".gitignore and ".gitattributes".
| * caa16d4 Start branch "feat/repoSetup".
|/  
* eea3281 (root) Create a new repository.
```

# The root and top Branches

The branches named `root` and `top` help to keep a clear picture of the repository as a whole. Without the clutter of unfinished features and work in progress, the goal of all branches merging to a single mainline is visible.

`root` is the first commit, should have no project files (only add the `.gitbranch` file), and never moves. However, it is still helpful to have this branch in your repository for filtering using the `--contains=root` argument that several commands accept. Also, it is sometimes helpful to create separate trees in your repository and `root` is the best way to start an empty "orphan branch".

`top` moves with every release (details in a later section). It should be made the "default branch" in the central remote repository (e.g. in Azure DevOps). It is the branch that is checked out when someone clones the repository for the first time, and should contain the "best" version of the code base. 

# Where to Commit New Work and Planning Ahead with top

Don't commit new work on top!

In the following log graph, there is a branch named `feature/newFeature` which is currently being worked on. However, the section marked with `»` symbols contains several commits with messages like "End branch ...", "Merge ...", and "top". The marked section is only the "planned" way that the branches eventually merge, and that is why it appears to be "ahead" of the feature being developed.

When you start on the next feature (e.g. `feature/newFeature`), work in progress (e.g. `u/user.name/wip`), or release branch (e.g. `u/user.name/semver/minor`), it should start based on existing work (in this example `semver/minor`), not planned future merges.

In the example below, the feature branch was created by first checking out the most recent release:
`git checkout semver/minor`
Then, creating and switching to the new feature branch:
`git checkout -b feature/newFeature`
Now, when new work is committed, it goes onto `feature/newFeature`.

```
» * 77534d7 (top) top
» *   676aac5 Merge branch semver/major.
» |\  
» | * 68ea9fe End branch "semver/major".
» | *   5ff7c11 Merge branch "semver/minor".
» | |\  
» | | * a115621 End branch "semver/minor".
  | | | * f07cf19 (feature/newFeature) wip
  | | | * 4a5131f wip
  | | | * 77d858d wip
  | | | * f07cf19 Start branch "feature/newFeature".
  | | |/  
  | | * c955d70 (stage/production, semver/minor, tag: semver/0.2.0) 0.2.0
  | | * 1a3b91e Changes
  | | *   5ff7c11 Merge branch "semver/patch".
  | | |\  
  | | | * 3dc530a End branch "semver/patch".
  | | | * 51bba9e (tag: semver/0.1.1) 0.1.1
  | | | * 1a3b91e Changes
  | | | * d20a5c1 Changes
  | | | * f07cf19 Start branch "semver/patch".
  | | |/  
  | | * c955d70 (tag: semver/0.1.0) 0.1.0
  | | * 4a5131f Changes
  | | * 46d3bff Changes
  | | * 77d858d Change "name" and "description" in "project.json".
  | | * a421cd8 (semver/minor) Start branch "semver/minor".
  | |/  
  | * a82ec76 (semver/major) Start branch "semver/major".
  |/  
  * b70ccd4 Non merge branch parent.
  *   62c16cf Merge branch "feat/repoSetup".
  |\  
  | * a38e260 End branch "feat/repoSetup".
  | * d20a5c1 Create blank UiPath Process and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
  | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

# Merge Finished Work

After a branch is complete, it may only be merged back into the same branch it is based on.

Often, no other work will have been done on the base branch. By default, `git merge` tries to "fast forward", which does not create a "merge commit". The option `--no-ff` must be used to tell git to create the proper merge commit we want in our log graph.

In this example, we will merge `feature/newFeature` back into `semver/minor`.
First check out the branch to merge the feature into.
`git checkout semver/minor`
Then merge in the feature.
`git merge --no-ff feature/newFeature`

However, our graph doesn't look right. `top` is no longer the first commit listed, and there is a "twist" in the graph (marked with `»`) where the line of `semver/major` falls to the right hand side of the line for `semver/minor` that we just updated. In the next step, we will update `top` to show how the new work is planned to be integrated into the mainline.

```
  *   75d58b2 (semver/minor) Merge branch feature/newFeature
  |\
  | * 65949a4 (feature/newFeature) End branch feature/newFeature.
  | * caa73a9 wip
  | * f07cf19 wip
  | * 4a5131f wip
  | * 77d858d wip
  | * 32f17c8 Start branch feature/newFeature
  |/
  | * 44d86c4 (top) top
  | *   e9c9f1b Merge branch semver/major.
  | |\
  | | * e0e61c5 End branch semver/major.
  | | *   2ad6434 Merge branch semver/minor.
  | | |\
» | | | * ce52be1 End branch semver/minor.
» | |_|/
» |/| |
» * | | 13e081a (tag: semver/0.2.0) 0.2.0
  * | | 8f7c14b Changes
  * | |   496d993 Merge branch semver/patch.
  |\ \ \
  | * | | bd3e723 End branch semver/patch.
  | * | | ba591e4 (tag: semver/0.1.1, semver/patch) 0.1.1
  | * | | c0321ad Changes
  | * | | 566c233 Start branch semver/patch.
  |/ / /
  * | | cd7ddbd (tag: semver/0.1.0) 0.1.0
  * | | 78bf981 Changes
  * | | c9a45ec Changes
  * | | ea67ddf Change "name" and "description" in "project.json".
  * | | 0614f74 Start branch semver/minor
» | |/
» |/|
» * | 02e45a2 Start branch semver/major
  |/  
  * b70ccd4 Non merge branch parent.
  *   62c16cf Merge branch "feat/repoSetup".
  |\  
  | * a38e260 End branch "feat/repoSetup".
  | * d20a5c1 Create blank UiPath Process and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
  | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

# Keep Top up to Date

After completed work is merged, rebase `top` to restore the log graph to show how all branches merge to a single mainline.

For this example, we'll use the following command.
`git rebase -i --rebase-merges --onto=semver/minor semver/minor`

The `-i` flag stands for "interactive", and causes your text editor to open with the steps to be done during the rebase (seen below). If the steps are correct, simply close the editor to begin. The `--rebase-merges` flag is required to reconstruct the merge commits, the default behavior of `rebase` is to flatten all selected commits into a single line. The `--onto=targetBranch upstreamBranch` arguments tell git which commits to use and where to put them.

_Tip:_ [Use Notepad++ as Your Text Editor in Git Bash](../../Use-Notepad++-as-Your-Text-Editor-in-Git-Bash.html)

The following is an example of what may be seen in the editor when using the `-i` flag. _Note:_ Comments at the bottom are truncated.

```
label onto

# Branch Merge-branch-semver-minor-
reset onto
pick ce52be1 End branch semver/minor.
label Merge-branch-semver-minor-

# Branch Merge-branch-semver-major-
reset 02e45a2 # Start branch semver/major
merge -C 2ad6434 Merge-branch-semver-minor- # Merge branch semver/minor.
pick e0e61c5 End branch semver/major.
label Merge-branch-semver-major-

reset 2486da5 # Non merge branch parent.
merge -C e9c9f1b Merge-branch-semver-major- # Merge branch semver/major.
pick 44d86c4 top
pick ac62dd5 Renormalize.

# Rebase 75d58b2..ac62dd5 onto e0e61c5 (13 commands)
#
# Commands:
# ... 
```

The following log graph now looks very similar to the section above, [Where to Commit New Work](./Mainline-with-Semantic-Versioning.html#where-to-commit-new-work-and-planning-ahead-with-top), but now the completed work is no longer diverging away, but merged into `semver/minor`.

```
  * 79cb5ef (top) top
  *   eef9045 Merge branch semver/major.
  |\  
  | * 12b42ff End branch "semver/major".
  | *   ac14423 Merge branch "semver/minor".
  | |\  
  | | * 05f91d2 End branch "semver/minor".
» | | *   75d58b2 2021-01-10 (semver/minor) Merge branch feature/newFeature
» | | |\
» | | | * 65949a4 2021-01-10 (feature/newFeature) End branch feature/newFeature.
  | | | * f07cf19 (feature/newFeature) wip
  | | | * 4a5131f wip
  | | | * 77d858d wip
  | | | * f07cf19 Start branch "feature/newFeature".
  | | |/  
  | | * c955d70 (stage/production, semver/minor, tag: semver/0.2.0) 0.2.0
  | | * 1a3b91e Changes
  | | *   5ff7c11 Merge branch "semver/patch".
  | | |\  
  | | | * 3dc530a End branch "semver/patch".
  | | | * 51bba9e (tag: semver/0.1.1) 0.1.1
  | | | * 1a3b91e Changes
  | | | * d20a5c1 Changes
  | | | * f07cf19 Start branch "semver/patch".
  | | |/  
  | | * c955d70 (tag: semver/0.1.0) 0.1.0
  | | * 4a5131f Changes
  | | * 46d3bff Changes
  | | * 77d858d Change "name" and "description" in "project.json".
  | | * a421cd8 (semver/minor) Start branch "semver/minor".
  | |/  
  | * a82ec76 (semver/major) Start branch "semver/major".
  |/  
  * b70ccd4 Non merge branch parent.
  *   62c16cf Merge branch "feat/repoSetup".
  |\  
  | * a38e260 End branch "feat/repoSetup".
  | * d20a5c1 Create blank UiPath Process and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
  | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

# Next

[The gitbranch File - Start & End Branch Commits](./The-gitbranch-File-Start-&-End-Branch-Commits.html)
