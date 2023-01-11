---
title: Test Code Isolation Branches
layout: default
nav_order: 2
parent: Branching Strategies
---
# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc }

- TOC
{:toc}

# Test Code Isolation Branches

## Pattern
`test/...` 

## Examples
`test/semver/minor`
`test/feature/newFeature`

## Description

UiPath publishes all project files in the process distribution package. It is advisable to prevent test code from being published into production environments. Test code can be added into the project folder only at testing time by use of a `test/...` branch. This also enables some advanced testing techniques like workflow mocking.

## Example Log Graph

Example log graph showing a test branch (marked with `»`).

```
  * 77534d7 (top) top
  *   676aac5 Merge branch semver/major.
  |\  
  | * 68ea9fe End branch "semver/major".
  | * 6196a9e (stage/production, tag: semver/1.0.0) 1.0.0
  | *   5ff7c11 Merge branch "semver/minor".
  | |\  
  | | * a115621 End branch "semver/minor".
  | | *   5ff7c11 Merge branch "semver/patch".
  | | |\  
  | | | * 3dc530a End branch "semver/patch".
» | | | | * a9b67a2 (test) Implement test workflow.
» | | | | * 902bb6e Mock "DateTimeNow.xaml".
» | | | | * 3f45129 Update project name and description.
» | | | | * 7c15621 Start test branch.
  | | | |/
  | | | * 51bba9e (tag: semver/0.1.1) 0.1.1
  | | | * 1a3b91e Changes
  | | | * 1a3b91e Changes
  | | | * f07cf19 (semver/minor) Start branch "semver/patch".
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

# Working With a Test Branch

__TODO:__ Explain how to update a test branch with the command `git rebase -i --onto=feature/newFeature feature/newFeature`

__TODO:__ Explain dropping a commit from the rebase after using `git commit --amend`

__TODO:__ Test branches are always "diverging", never merged.

__TODO:__ Test branches can be duplicated for different branches under active development.

__TODO:__ How to track new changes to a test branch, and combine them when branches merge.

# Workflow Mocking

__TODO:__ Overwrite a workflow to behave differently during testing. Use case example: Force the value of `DateTime.Now` to be a value configured for a test case.

# Next

[Contributor Branch Namespaces](./Contributor-Branch-Namespaces.html)
