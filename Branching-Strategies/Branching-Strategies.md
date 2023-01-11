---
title: Branching Strategies
layout: default
nav_order: 7
has_children: true
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Trees vs Lanes

The strategies below are __tree__ based strategies, not __lane__ based strategies. Many lane based git diagrams you may have seen before are visually depicted horizontally (unlike actual git log graphs which are always vertical) and do not fit naturally with the data structure that git uses internally (a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree){:target="_blank"}). Work committed in git does not tend to stay in lanes, but to diverge farther with each commit on a branch. Crossing lines in git diagrams generally indicate [anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern){:target="_blank"} that make the repository difficult to maintain. The strategies below help minimize divergence and eliminate crossing merges.

![badGitDiagram.png](/{{site.github.repository_name}}/assets/images/badGitDiagram.png)

# Strategies

## [Mainline with Semantic Versioning](./Mainline-with-Semantic-Versioning.html)

**semver branches**

`semver/major`
`semver/minor`
`semver/patch`

Disambiguate multiple same-level branches (see [Example Hotfix](./Mainline-with-Semantic-Versioning/Example-Hotfix-Publishing-A-Patch-on-a-Major-Version.html)):
`semver/patch-1.0.x`

**semver tags**

production release:
`semver/1.0.0`

pre-release:
`semver/1.0.0-wip001`

**deployment stage branches**

pattern:
`stage/...`

examples:
`stage/development`
`stage/qa`
`stage/beta`
`stage/production`

Optionally, trigger pipelines with separate branches and only update stage branches after deployment is successful.
`stage/trigger/...`
`stage/trigger/development`
`stage/trigger/qa`
`stage/trigger/beta`
`stage/trigger/production`

__Feature Branches__

pattern:
`feature/...`

examples:
`feature/newFeature`

merge plans:
`feature/mergePlan/...`
`feature/mergePlan/newFeature`

__Release Branches__

pattern:
`release/...`

examples:
`release/1.0.0`

__Strategy Details__

* [Mainline with Semantic Versioning](./Mainline-with-Semantic-Versioning.html)
* ##### [The gitbranch File - Start & End Branch Commits](./Mainline-with-Semantic-Versioning/The-gitbranch-File-Start-&-End-Branch-Commits.html)
* ##### [Mainline Commits Must Pass Tests](./Mainline-with-Semantic-Versioning/Mainline-Commits-Must-Pass-Tests.html)
* ##### [Feature Branches](./Mainline-with-Semantic-Versioning/Feature-Branches.html)
* ##### [Don't Span the Gap - Restart Branches After Merging](./Mainline-with-Semantic-Versioning/Don't-Span-the-Gap-Restart-Branches-After-Merging.html)
* ##### [Plan Ahead Your Merge](./Mainline-with-Semantic-Versioning/Plan-Ahead-Your-Merge.html)
* ##### [Example Hotfix - Publishing A Patch on a Major Version](./Mainline-with-Semantic-Versioning/Example-Hotfix-Publishing-A-Patch-on-a-Major-Version.html)

## [Test Code Isolation Branches](./Test-Code-Isolation-Branches.html)

pattern:
`test/...`

examples:
`test/semver/minor`
`test/feature/newFeature`

## [Contributor Branch Namespaces](./Contributor-Branch-Namespaces.html)

pattern:
`u/user.name/...` 

examples:
`u/user.name/semver/minor`
`u/user.name/feature/newFeature`
`u/user.name/wip`

## [Other Branch Namespace](./Other-Branch-Namespace.html)

pattern:
`other/...`

examples:
`other/archive/feature/newFeature-OLD_VERSION`

# Next

[Mainline with Semantic Versioning](./Mainline-with-Semantic-Versioning/Mainline-with-Semantic-Versioning.html)
