---
title: Example Hotfix - Publishing A Patch on a Major Version
layout: default
nav_order: 7
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Example Hotfix: Publishing A Patch on a Major Version

In cases like a hotfix, disambiguate multiple semver branches of the same level with a name like:
`semver/patch-1.0.x`
But in [The gitbranch File - Start & End Branch Commits](./The-gitbranch-File-Start-&-End-Branch-Commits.html), use only the standard `semver/patch` portion of the name.

__TODO:__ Write example.

__TODO:__ If hotfix is needed on lower level semver branches, use `cherry-pick`, not `merge`.

# Next

[Test Code Isolation Branches](../Test-Code-Isolation-Branches.html)
