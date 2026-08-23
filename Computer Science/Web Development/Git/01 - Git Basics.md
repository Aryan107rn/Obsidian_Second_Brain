---
tags: [git, version-control, command-line]
aliases: [Git Basics, git setup, git commands]
created: 2026-08-16
---

# 01 - Git Basics

## What is Git?
Git is a **distributed version control system** — it tracks changes to files over time, letting you save snapshots (commits), go back to any previous state, and collaborate without overwriting each other's work.

**Distributed** means every clone has the *entire* project history, not just the latest files — unlike older centralized systems where only one server held the full history.

## Setup
```
git init                             # start a new repo
git clone url                        # copy an existing remote repo

git config --global user.name "You"
git config --global user.email "you@x.com"
```

## The Four Stages
```
Working dir  --git add-->  Staging area  --git commit-->  Local repo  --git push-->  Remote
```
- **Working directory** — your actual edited files, as they sit on disk right now
- **Staging area (Index)** — files marked with `git add`, queued for the *next* commit
- **Local repo (`.git`)** — committed snapshots (full history), stored on your machine
- **Remote** — the hosted copy (e.g. GitHub), synced via `push`/`pull`

**Why a staging area exists at all:** it lets you build a commit out of only *some* of your changes — e.g. you fixed two unrelated bugs in the same session, and want them as two separate, clean commits instead of one messy one. `git add` picks what goes in next.

## Daily Flow
```
git status                 # what changed?
git add file                # stage one file
git add .                   # stage everything
git commit -m "message"     # save a snapshot
git push                    # send commits to remote
git pull                    # fetch + merge from remote
```

## Viewing History
```
git log                         # full commit history
git log --oneline --graph        # compact visual history
git diff                         # unstaged changes
git show commit_hash              # details of one commit
```

## Remote
```
git remote add origin url
git fetch                          # download changes, don't merge yet
git push -u origin main             # first push; sets upstream tracking
```
**Fetch vs pull:** `fetch` downloads without merging, so you can review before combining. `pull` = `fetch` + `merge` in one step — convenient, but skips the review opportunity.

## Key Concepts
- **Commit** — an immutable snapshot of the whole repo at a point in time, identified by a unique hash (not a diff — Git actually stores full snapshots internally, though it's smart about not duplicating unchanged files).
- **HEAD** — a pointer to your current position, usually the tip of the branch you're on. Moving between commits/branches moves HEAD.

## Common mistakes
- Running `git add .` without checking `git status` first — accidentally staging files you didn't mean to commit (build artifacts, secrets, unrelated edits).
- Writing vague commit messages ("fix stuff") — makes `git log` useless later when you need to find when/why something changed.
- Forgetting `git pull` before starting work — leads to avoidable merge conflicts when your local branch has diverged from remote.

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Branching]]
- [[Git Undoing Changes]]
