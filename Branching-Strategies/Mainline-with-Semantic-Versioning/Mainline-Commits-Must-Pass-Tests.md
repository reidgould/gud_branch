---
title: Mainline Commits Must Pass Tests
layout: default
nav_order: 2
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Mainline Commits Must Pass Tests

Every commit on the mainline (any one of the `semver/...` branches) must successfully build and pass all tests.

Work in progress should be committed to a [feature branch](./Feature-Branches.html) (for example `feature/newFeature`) or a [contributor's work in progress branch](../Contributor-Branch-Namespaces.html) (for example `u/user.name/semver/minor`).
When complete, feature branches must merged in order to be included in a release on a semver branch.
Contributor branches must be rebased and squashed before being pulled into the target branch.

Single commits of complete work are allowed on a mainline branch without a feature.

# Next

[Feature Branches](./Feature-Branches.html)