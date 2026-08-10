---
tags: [git, version-control, command-line]
aliases: [git stash]
created: 2026-08-08
---

# Git Stash

**Stash** temporarily saves uncommitted work so you can switch tasks without committing half-finished changes.

## Why It Exists

You're mid-feature but need to fix an urgent bug on `main`. Committing unfinished work just to switch branches pollutes history — stash parks it instead.

## Core Commands

```
git stash                          # save current changes
git stash push -m "label"          # save with a description
git stash list                     # see all stashes
git stash apply                    # restore most recent, keep it in the list
git stash apply stash@{1}          # restore a specific stash
git stash pop                      # restore + remove from the list
git stash drop stash@{0}           # delete one stash
git stash clear                    # delete all stashes
```

`apply` vs `pop`: `apply` keeps the stash around in case you need it again; `pop` applies it and removes it.

## Real-World Example

```
git stash push -m "half-done login form"
git switch main
git switch -c hotfix/typo
# fix, commit, merge...
git switch feature/login
git stash pop
```

## Related Concepts
- [[Git and GitHub]]
- [[Git Rebase]]
- [[Merge Conflicts]]
