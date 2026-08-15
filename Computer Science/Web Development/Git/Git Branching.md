---
tags: [git, version-control, command-line]
aliases: [git branches, branching]
created: 2026-08-16
---

# Git Branching

A **branch** is an independent line of development — a way to work on a feature without touching `main` until it's ready.

![[git-feature-branch-workflow.png|860]]

## Commands
```
git branch                 # list branches
git branch name             # create a branch
git checkout name           # switch to a branch
git checkout -b name        # create and switch in one step
git merge name               # merge a branch into current branch
git branch -d name           # delete a branch (safe — blocks if unmerged)
git branch -D name           # force delete, even if unmerged
```

## Why branch instead of committing straight to `main`?
`main` should always be in a working, deployable state. A branch lets you experiment, break things, and commit half-finished work without risking that.

## Merging
```
git checkout main
git merge feature-branch
```
If the same lines were changed on both branches, Git can't auto-combine them — see [[Merge Conflicts]].

## Related Concepts
- [[Git and GitHub]] — overview
- [[Git Basics]]
- [[Git Rebase]] — alternative to merging, produces linear history
- [[Merge Conflicts]]
