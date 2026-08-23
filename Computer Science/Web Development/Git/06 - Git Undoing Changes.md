---
tags: [git, version-control, command-line]
aliases: [Git Undoing Changes, git restore, git reset, git revert]
created: 2026-08-16
---

# 06 - Git Undoing Changes

Git has different undo tools depending on **how far** a change has progressed — unstaged, staged, committed locally, or already pushed to a shared remote. Picking the right one matters: some rewrite history (dangerous if shared), some don't.

```
git restore file                   # discard unstaged edits to one file
git restore .                      # discard all unstaged edits
git restore --staged file          # unstage a file (keeps the edits)
git reset --soft HEAD~1            # undo last commit, keep changes staged
git reset --hard HEAD~1            # undo last commit, discard changes entirely
git revert HEAD                    # new commit that undoes the last one
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
`reset --hard` **rewrites your local history** — safe only on commits **nobody else has pulled**. Once a commit is pushed and shared, use `git revert` instead: it adds a *new* commit that undoes the change, rather than erasing history other people may already depend on. This mirrors [[Git Rebase]]'s golden rule — never destructively rewrite history that other people have already based work on.

## `reset` soft vs mixed vs hard (the full picture)
- `--soft` — moves HEAD back, keeps changes **staged** (ready to re-commit).
- `--mixed` (default if you omit the flag) — moves HEAD back, keeps changes **unstaged** (in working directory, but not staged).
- `--hard` — moves HEAD back, **discards changes entirely**. Unrecoverable through normal means (technically recoverable briefly via `git reflog`, but don't rely on it).

## Common mistakes
- Using `reset --hard` out of habit without checking if the commit was already pushed — can silently discard teammates' basis for their own work if force-pushed afterward.
- Confusing `restore` (working-directory/staging level) with `reset` (commit level) — they operate at different stages of the four-stage flow.
- Forgetting `git reflog` exists as a last-resort safety net — even after a `reset --hard`, the old commit is usually still recoverable for a while via reflog, since Git doesn't immediately garbage-collect it.

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Basics]]
- [[Git Stash]] — for parking work temporarily instead of undoing it
- [[Git Rebase]] — shares the same "don't rewrite shared history" principle
