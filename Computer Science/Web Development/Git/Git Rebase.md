---
tags: [git, version-control, command-line]
aliases: [rebase, git rebase]
created: 2026-08-08
---

# Git Rebase

**Rebase** replays your branch's commits on top of another branch, producing a linear history instead of a merge commit.

## Merge vs Rebase

Starting point: branch diverged from `main` at commit B. Feature branch has C, D; `main` moved on to E.

**Merge** keeps the fork and adds a merge commit M:
```
   C─D
  /    \
A─B─E───M
```

**Rebase** replays C and D on top of E as new commits C', D' — a straight line, no merge commit:
```
A─B─E─C'─D'
```

| | Merge | Rebase |
|---|---|---|
| History | Preserves branch structure | Linear, cleaner |
| Extra commit | Yes (merge commit) | No |
| Safe on shared branches | Yes | No — rewrites commit hashes |

## Commands

```
git switch feature
git rebase main                          # replay feature's commits on main

# keep a feature branch in sync with updated main
git fetch origin
git rebase origin/main

# after rebasing a branch you've already pushed
git push --force-with-lease origin feature

# if it goes wrong
git rebase --abort
```

## The Golden Rule

**Never rebase commits that have already been pushed to a shared branch.** Rebase rewrites commit history (new hashes for C', D'), so anyone who already pulled the old commits will get conflicting history. Safe to rebase: local branches only you work on, before opening a PR.

## When to Use It

- Cleaning up your branch before opening a pull request
- Syncing a feature branch with an updated `main`
- On branches nobody else has pulled from

## Related Concepts
- [[Git and GitHub]]
- [[Merge Conflicts]] — rebase can also produce these, resolved the same way
- [[Git Stash]]
