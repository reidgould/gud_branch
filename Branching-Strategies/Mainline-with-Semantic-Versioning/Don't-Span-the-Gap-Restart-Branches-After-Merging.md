---
title: Don't Span the Gap - Restart Branches After Merging
layout: default
nav_order: 4
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Restarting a Semver Branch After a Release

When making a release, all lower level semver branches and features (which are to be included in that release) must be ended and merged.

When more work needs to be done at a lower semver level, for example `semver/minor`, start a new branch named `semver/minor` after the release.

The following log graph shows two `semver/minor` branches (marked with `»`) before and after a release on `semver/major` tagged `semver/1.0.0`. It also shows a branch `feature/newFeature` that was not completed in time to be included in the release.

```
  * 77534d7 (top) top
  *   676aac5 Merge branch semver/major.
  |\  
  | * 68ea9fe End branch "semver/major".
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
» | | * a115621 End branch "semver/minor".
» | | * c955d70 (semver/minor, tag: semver/1.1.0) 1.1.0
» | | * 4a5131f Changes
» | | * 46d3bff Changes
» | | * a421cd8 (semver/minor) Start branch "semver/minor".
  | |/  
  | * 6196a9e (semver/major) (stage/production, tag: semver/1.0.0) 1.0.0
  | * f2ddc34 Changes
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
» | | * a115621 End branch "semver/minor".
» | | *   5ff7c11 Merge branch "semver/patch".
» | | | * f4dd5f9 (feature/newFeature) Changes
» | | | * 1a3b91e Changes
» | | | * f07cf19 Start branch "feature/newFeature".
» | | |/  
» | | * c955d70 (tag: semver/0.1.0) 0.1.0
» | | * 4a5131f Changes
» | | * 46d3bff Changes
» | | * 77d858d Change "name" and "description" in "project.json".
» | | * a421cd8 Start branch "semver/minor".
  | |/  
  | * a82ec76 Start branch "semver/major".
  |/  
  * b70ccd4 Non merge branch parent.
  *   62c16cf Merge branch "feat/repoSetup".
  |\  
  | * a38e260 End branch "feat/repoSetup".
  | * d20a5c1 Create blank project and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
  | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

# Updating a Feature Branch After a Release

## __Incorrectly__ Spanning Over a Higher Level Release

Feature branches may not merge in a way that spans a gap in mainline branching.

In this example, if `feature/newFeature` is now merged into `semver/minor`, it will incorrectly span over release `semver/1.0.0` (marked with `»`).

The following log graph is _faked_ for easier understanding! If you do this, the graph git shows will be _much_ uglier.

```
» Warning, incorrect example!

  * 77534d7 (top) top
  *   676aac5 Merge branch semver/major.
  |\  
  | * 68ea9fe End branch "semver/major".
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
  | | * a115621 End branch "semver/minor".
  | | * c955d70 (semver/minor, tag: semver/1.1.0) 1.1.0
» | | *   5ff7c11 Merge branch "feature/newFeature".
» | | |\  
» | | | * 3dc530a End branch "feature/newFeature".
» | | * | 4a5131f Changes
» | | * | 46d3bff Changes
» | | * | a421cd8 Start branch "semver/minor".
» | |/  | 
» | *   | 6196a9e (semver/major) (stage/production, tag: semver/1.0.0) 1.0.0
» | *   | f2ddc34 Changes
» | *   | 5ff7c11 Merge branch "semver/minor".
» | |\  | 
» | | * | a115621 End branch "semver/minor".
» | | | * 1a3b91e Changes
» | | | * 1a3b91e Changes
» | | | * f07cf19 Start branch "feature/newFeature".
  | | |/  
  | | * c955d70 (tag: semver/0.1.0) 0.1.0
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
  | * d20a5c1 Create blank project and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
  | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

## Correctly Rebasing Onto The New Mainline

Instead, rebase `feature/newFeature` onto the new `semver/minor`.

The following log graph shows `feature/newFeature` after being rebased (marked with `»`) using the following commands.
`git checkout feature/newFeature`
`git rebase -i --onto=semver/minor c955d70`
There may be conflicts that need to be resolved during the rebase.

After the rebase, `feature/newFeature` can be merged back into `semver/minor` because `feature/newFeature` is based on it.

```
  * 77534d7 (top) top
  *   676aac5 Merge branch semver/major.
  |\  
  | * 68ea9fe End branch "semver/major".
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
  | | * a115621 End branch "semver/minor".
» | | | * e2dba39 (feature/newFeature) Changes
» | | | * 178b6ff Changes
» | | | * 631b75f Start branch "feature/newFeature".
  | | |/  
  | | * c955d70 (semver/minor, tag: semver/1.1.0) 1.1.0
  | | * 4a5131f Changes
  | | * 46d3bff Changes
  | | * a421cd8 (semver/minor) Start branch "semver/minor".
  | |/  
  | * 6196a9e (semver/major) (stage/production, tag: semver/1.0.0) 1.0.0
  | * f2ddc34 Changes
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
  | | * a115621 End branch "semver/minor".
  | | *   5ff7c11 Merge branch "semver/patch".
  | | * c955d70 (tag: semver/0.1.0) 0.1.0
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
  | * d20a5c1 Create blank project and add standard dependencies.
  | * fd45a8f Create ".gitignore and ".gitattributes".
  | * caa16d4 Start branch "feat/repoSetup".
  |/  
  * eea3281 (root) Create a new repository.
```

# Next

[Plan Ahead Your Merge](./Plan-Ahead-Your-Merge.html)