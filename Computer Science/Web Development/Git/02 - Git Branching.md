---
tags: [git, version-control, command-line]
aliases: [Git Branching, git branches, branching]
created: 2026-08-16
---

# 02 - Git Branching

## What is a branch?
A **branch** is an independent line of development — a movable pointer to a commit, letting you work on a feature (or fix, or experiment) without touching `main` until it's ready.

![[git-feature-branch-workflow.png|860]]

## Why branch instead of committing straight to `main`?
`main` should always be in a working, deployable state. A branch lets you experiment, break things, and commit half-finished work in small increments — without risking `main`'s stability for everyone else relying on it.

## Commands
```
git branch                  # list branches
git branch name              # create a branch
git checkout name            # switch to a branch
git checkout -b name         # create and switch in one step
git merge name                # merge a branch into current branch
git branch -d name            # delete a branch (safe — blocks if unmerged)
git branch -D name            # force delete, even if unmerged
```

`git switch name` is the newer, clearer alternative to `git checkout name` for switching branches (checkout historically did too many unrelated things — switching branches AND restoring files — so `switch`/`restore` were split out to reduce confusion).

## Merging
```
git checkout main
git merge feature-branch
```
Git tries to auto-combine the two branches' changes. If the same lines were changed on both, it can't decide automatically — see [[Merge Conflicts]].

**Fast-forward vs three-way merge:** if `main` hasn't moved since the branch was created, Git just moves `main`'s pointer forward (fast-forward — no merge commit needed). If both branches have new commits, Git creates a merge commit joining both histories.

## Common mistakes
- Working directly on `main` for anything non-trivial — makes it hard to abandon a bad idea cleanly.
- Forgetting which branch you're on before committing (`git status` always shows this) — leads to commits landing on the wrong branch.
- Long-lived branches that drift far from `main` — the longer a branch lives without merging, the more likely a painful conflict becomes.

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Basics]]
- [[Git Rebase]] — alternative to merging, produces linear history
- [[Merge Conflicts]]
