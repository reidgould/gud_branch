---
title: Release Branches
layout: default
nav_order: 6
parent: Mainline with Semantic Versioning
grand_parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Release Branches

## Pattern
`release/...`

## Examples
`release/1.0.0`

## Description

In many projects, release branches are not needed.

Use release branches only if you have code changes that must occur in the project to prepare for a new version to be published, but which should not be included in the mainline.
Release branches are left to diverge from the mainline, and are not merged.

For example, if you have a value hard coded to point to a test system in the mainline, you can use a release branch to update it to point to production before a release.

Before using a release branch, consider if your code can be refactored to utilize parameters, environment variables, or other tools to provide the change in behavior of your software rather than using release branches which require repeated effort.

To save some release effort, you may be able to use `git rebase` to replay the changes from the previous release rather than having to perform them manually each time.


## Example

__TODO:__ Write example. 

# Next

[Example Hotfix - Publishing A Patch on a Major Version](./Example-Hotfix-Publishing-A-Patch-on-a-Major-Version.html)
