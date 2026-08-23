---
tags: [git, version-control, command-line]
aliases: [Git Stash, git stash]
created: 2026-08-08
---

# 05 - Git Stash

## Why it exists
You're mid-feature but need to fix an urgent bug on `main`. Committing unfinished, half-working code just to switch branches pollutes your history with meaningless "WIP" commits. **Stash** parks uncommitted work aside instead, letting you switch branches with a clean working directory.

## Core Commands
```
git stash                            # save current changes
git stash push -m "label"            # save with a description
git stash list                       # see all stashes
git stash apply                      # restore most recent, keep it in the list
git stash apply stash@{1}            # restore a specific stash
git stash pop                        # restore + remove from the list
git stash drop stash@{0}             # delete one stash
git stash clear                      # delete all stashes
```

**`apply` vs `pop`:** `apply` restores the changes but keeps the stash in the list (useful if you might need to apply it again, e.g. to a different branch). `pop` restores and immediately removes it — use this when you're done with the stash for good.

## Real-World Example
```
git stash push -m "half-done login form"
git switch main
git switch -c hotfix/typo

# fix, commit, merge...

git switch feature/login
git stash pop
```

## Common mistakes
- Forgetting stashes exist — `git stash list` can accumulate forgotten entries over weeks; check it periodically.
- Using plain `git stash` repeatedly without labels (`-m`) — makes it hard to tell stashes apart later.
- Stashing, then making conflicting edits on the same lines before popping — can cause a stash-pop conflict, resolved the same way as a merge conflict.

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Rebase]]
- [[Merge Conflicts]]
