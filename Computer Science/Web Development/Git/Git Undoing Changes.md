---
tags: [git, version-control, command-line]
aliases: [git restore, git reset, git revert]
created: 2026-08-16
---

# Git Undoing Changes

Git has different undo tools depending on how far a change has gone — unstaged, staged, committed locally, or already pushed.

```
git restore file                  # discard unstaged edits to one file
git restore .                     # discard all unstaged edits
git restore --staged file         # unstage a file (keeps the edits)
git reset --soft HEAD~1           # undo last commit, keep changes staged
git reset --hard HEAD~1           # undo last commit, discard changes entirely
git revert HEAD                   # new commit that undoes the last one
```

## Which one do I use?

| Situation | Command |
|---|---|
| Edited a file, want to discard the edit | `git restore file` |
| Staged a file by mistake | `git restore --staged file` |
| Local commit is wrong, keep the edits | `git reset --soft HEAD~1` |
| Local commit is wrong, discard everything | `git reset --hard HEAD~1` |
| Commit is already pushed / shared | `git revert HEAD` |

## The Rule

`reset --hard` rewrites your local history — safe only on commits **nobody else has pulled**. Once a commit is pushed and shared, use `git revert` instead: it adds a *new* commit that undoes the change, rather than erasing history other people may already depend on.

## Related Concepts
- [[Git and GitHub]] — overview
- [[Git Basics]]
- [[Git Stash]] — for parking work temporarily instead of undoing it
