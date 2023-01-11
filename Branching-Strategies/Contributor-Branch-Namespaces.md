---
title: Contributor Branch Namespaces
layout: default
nav_order: 3
parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Contributor Branch Namespaces

## Pattern
`u/user.name/...` 

## Examples
`u/user.name/semver/minor`
`u/user.name/feature/newFeature`
`u/user.name/wip`

## Description

### Contribute to Protected Branches

For any branch that a contributor does not have access to, push work that needs approval to a branch like `u/user.name/branch/name` then submit a pull request for the branch to be "fast forward" merged to the new work.

### Contribute to The Mainline

A contributor's mainline branch may be named something like `u/user.name/semver/minor`.

One of the rules of the mainline is that [Mainline Commits Must Pass Tests](./Mainline-with-Semantic-Versioning/Mainline-Commits-Must-Pass-Tests.html). However the work may still take some time to complete.

Use a contributor branch to commit and push daily work in progress. When the work is complete, use `git rebase --squash` to combine them into a single acceptable commit. Then, submit a pull request to "fast forward" merge the mainline branch to the contributor's finished work.

### Contribute to A Feature Branch

A contributor's feature branch may be named something like `u/user.name/feature/newFeature`.

For a single developer working on a feature branch they have access to push to, don't use a contributor branch. Instead, commit directly to the feature.

For multiple developers working on the same feature, each should commit and push their own branch so that they can make progress separately. Each should commit and push daily work in progress to be backed up continually. When the developers need to combine their work, one must rebase onto the other in order to maintain a single line of history for the feature. Then, submit a pull request to "fast forward" merge the feature to the latest commit.

# Next

[Other Branch Namespace](./Other-Branch-Namespace.html)
