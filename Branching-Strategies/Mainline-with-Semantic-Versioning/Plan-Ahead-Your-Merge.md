---
title: Plan Ahead Your Merge
layout: default
nav_order: 5
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Plan Ahead Merges for Ongoing Features

## Pattern
`feature/mergePlan/...`

## Examples
`feature/mergePlan/newFeature`

## Description

While [feature branches](./Feature-Branches.html) are being worked on, frequently perform "plan ahead" merges into the main line to ensure conflicts don’t become overwhelming.

"Plan ahead" branches should be temporary, and removed after the feature is merged into it's base branch.

## Example

### Set Up

Create a feature branch:
`git checkout semver/minor`
`git checkout -b feature/newFeature`

After doing some work, multiple commits may be created on `feature/newFeature`.
Time to do a "plan ahead" merge!


### First Plan Merge

Checkout the base branch.
`git checkout semver/minor`
Create a branch to save the merge.
`git checkout -b feature/mergePlan/newFeature`

Merge the feature.
`git merge --no-ff feature/newFeature`
Resolve any conflicts, then complete the merge.
`git merge --continue`

> Tip: 
> If your conflicts are easy enough to remember, the following "Additional" and "Final" techniques may be more complex than needed.
> You can just repeat the "First Plan" method each time you want to check your progress and your final merge can just be a normal merge.

### Additional Plan Merges

#### Update Plan With Feature Changes

As work continues, continue to update your merge plan.
To avoid the need to resolve the same conflicts as before, you can use the previous "plan ahead" merge.

`git checkout feature/mergePlan/newFeature`
Merge the feature.
`git merge --no-ff feature/newFeature`

Resolve any conflicts, then complete the merge.
`git merge --continue`

#### Update Plan With Mainline Changes

There may be changes on the Mainline that you want to include in your merge plan.
In this _special case_ for this _temporary branch_, merge the opposite direction from normal to update the "plan ahead" merge with changes from `semver/minor`.

`git checkout feature/mergePlan/newFeature`
Merge changes from the mainline.
`git merge --no-ff semver/minor`

Resolve any conflicts, then complete the merge.
`git merge --continue`

### Final Merge Into Base

Before the final merge, update the "plan ahead" merge branch with all changes from both feature and mainline. Any changes not merged into your plan will not be included in the final result.

Begin the merge normally.
`git checkout semver/minor`
`git merge --no-ff feature/newFeature`

Now all the conflicts that have accumulated show at once. 
Use `read-tree` to get all your resolved files from the "plan ahead" branch. For this to work, a clean index is required, so we'll just add all our _unresolved_ files (normally don't do this, we're about to overwrite them).
`git add --all`

Run the following `read-tree` command. 
The `-u` argument stands for "update", and causes the new files to also be written to your worktree (the default is to only write them to the index).
The `-m` argument stands for "merge" and is required, but because we only specified one tree no merging actually occurs.
`git read-tree -u -m feature/newFeature`

Confirm that all files contain your resolutions and are added to the index, then complete the merge.
`git merge --continue`

Delete the "plan ahead" branch.
`git branch -D feature/mergePlan/newFeature`

## Multiple Feature Branches

Don't cross merges. Decide which will be released first and rebase the second onto it's latest "plan ahead" merge. If the release order changes, the conflicts resolutions will need to be redone.

# Next

[Release Branches](./Release-Branches.html)