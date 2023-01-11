---
title: Feature Branches
layout: default
nav_order: 3
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Feature Branches

## Pattern
`feature/...`

## Examples
`feature/newFeature`

## Description

Create feature branches based on any branch of the mainline (any one of the `semver/...` branches).

The feature branch is allowed to have commits that are not complete, don't build, or don't pass tests.

The feature branch may only merge back into the mainline branch it was created from. Don't merge a feature branch in a way that spans a gap in a semver level, instead rebase the branch onto the branch it will later be merged into: [Don't Span the Gap - Restart Branches After Merging](./Don't-Span-the-Gap-Restart-Branches-After-Merging.html)

See [Mainline with Semantic Versioning](./Mainline-with-Semantic-Versioning.html) for examples of log graphs containing feature branches.

# Next

[Don't Span the Gap - Restart Branches After Merging](./Don't-Span-the-Gap-Restart-Branches-After-Merging.html)