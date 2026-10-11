---
tags: [git, version-control, command-line, recovery]
aliases: [Git Reflog, reflog, cherry-pick, git cherry-pick, git bisect, recover lost commits]
created: 2026-10-10
---

# 10 - Git Reflog, Cherry-pick & Recovery

Three power tools that turn "I broke it" into "fixed in a minute": **reflog** (undo almost anything), **cherry-pick** (copy a specific commit), and **bisect** (find which commit introduced a bug).

## `git reflog` — Git's safety net

### What it is
The **reflog** is a local log of every place `HEAD` (and each branch tip) has pointed — commits, checkouts, resets, rebases, merges. `git log` shows the history *reachable from a branch*; the reflog shows the history *of your own actions*, including commits that are no longer on any branch.

### Why it matters
Commands like `reset --hard`, a botched rebase, or deleting a branch with `-D` seem to destroy commits. They don't delete them right away — they just make them unreachable. The reflog still records them, so you can get them back.

```bash
git reflog
# a1b2c3d HEAD@{0}: reset: moving to HEAD~3
# 9f8e7d6 HEAD@{1}: commit: add payment validation   <-- the "lost" commit
# 4c5d6e7 HEAD@{2}: commit: add checkout page
```

### Recover from a bad `reset --hard`
```bash
git reflog                         # find the hash from before the mistake
git reset --hard 9f8e7d6           # move the branch back to it
# or, safer: keep current state and make a branch at the lost commit
git branch rescue 9f8e7d6
```

### Recover a deleted branch
```bash
git reflog                         # find the last commit the branch pointed to
git switch -c my-branch 9f8e7d6
```

### Limits
- Reflog is **local only** — it isn't pushed, and a fresh clone has none.
- Entries expire (by default ~90 days for reachable, ~30 days for unreachable commits), after which `git gc` may permanently delete them.
- It can only recover things that were **committed**. Uncommitted work wiped by `git restore`, `reset --hard`, or `git clean` was never recorded and is gone.

## `git cherry-pick` — copy one commit onto another branch

### What it is
Applies the changes introduced by a specific commit onto your current branch as a **new commit** (new hash, same change).

### Why it exists
Sometimes you need *one* fix from another branch without merging the whole branch — e.g. a bug fix made on `develop` that must also go to a `release` branch, or a commit made on the wrong branch.

```bash
git switch release/1.2
git cherry-pick a1b2c3d              # apply that commit here
git cherry-pick a1b2c3d b2c3d4e      # several commits
git cherry-pick A^..B                # a range, inclusive of A and B
git cherry-pick -x a1b2c3d           # append "(cherry picked from commit ...)" to the message
```
If it conflicts: fix files → `git add` → `git cherry-pick --continue` (or `--abort`).

### Example: commit landed on the wrong branch
```bash
git switch correct-branch
git cherry-pick <hash>               # copy it over
git switch wrong-branch
git reset --hard HEAD~1              # remove it from the wrong branch (only if not pushed!)
```

### Caveats
- It duplicates the change as a separate commit, so the same change exists twice in history; merging the two branches later can produce odd conflicts. Prefer merge/rebase for whole-branch integration.
- Don't use it as a substitute for proper branching — heavy cherry-picking is a sign the workflow needs fixing (see [[09 - Branching Strategies & PR Workflow|Branching Strategies]]).

## `git bisect` — binary-search for the commit that broke things
Instead of checking commits one by one, bisect halves the search range each step, finding the culprit in about log₂(n) tests.
```bash
git bisect start
git bisect bad                  # current commit is broken
git bisect good v1.0.0          # a commit/tag known to be fine
# Git checks out the midpoint; test it, then tell Git:
git bisect good                 # or: git bisect bad
# ...repeat until Git prints "<hash> is the first bad commit"
git bisect reset                # return to where you started
```
Automate with `git bisect run npm test` (Git runs the command at each step; exit code 0 = good).

## Handy recovery checklist
| Problem | Tool |
|---|---|
| Hard-reset away a commit | `git reflog` → `git reset --hard <hash>` |
| Deleted a branch too early | `git reflog` → `git switch -c <name> <hash>` |
| Commit made on the wrong branch | `cherry-pick` onto the right one, then remove from the wrong one |
| Need one fix on another branch | `git cherry-pick <hash>` |
| Don't know which commit introduced a bug | `git bisect` |
| Commit already pushed and bad | `git revert` (see [[06 - Git Undoing Changes]]; don't rewrite shared history) |

## Common mistakes
- Assuming `reset --hard` is irreversible for *committed* work — check reflog first. (It **is** irreversible for uncommitted edits.)
- Waiting too long — reflog entries expire.
- Cherry-picking whole feature branches commit by commit instead of merging.
- Forgetting to `git bisect reset` afterwards, leaving the repo in a detached-HEAD bisect state.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]]
- [[02 - Git Branching|Git Branching]] — detached HEAD
- [[04 - Git Rebase|Git Rebase]] — reflog rescues bad rebases
- [[06 - Git Undoing Changes|Git Undoing Changes]]
- [[09 - Branching Strategies & PR Workflow|Branching Strategies & PR Workflow]]
