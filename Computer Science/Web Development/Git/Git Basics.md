---
tags: [git, version-control, command-line]
aliases: [git setup, git commands]
created: 2026-08-16
---

# Git Basics

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
- **Working directory** — your actual edited files
- **Staging area** — files marked for the next commit
- **Local repo** — committed snapshots (history), on your machine
- **Remote** — the hosted copy (GitHub), synced via `push`/`pull`

## Daily Flow
```
git status                # what changed?
git add file               # stage one file
git add .                  # stage everything
git commit -m "message"    # save a snapshot
git push                   # send commits to remote
git pull                   # fetch + merge from remote
```

## Viewing History
```
git log                        # full commit history
git log --oneline --graph       # compact visual history
git diff                        # unstaged changes
git show commit_hash             # details of one commit
```

## Remote
```
git remote add origin url
git fetch                         # download changes, don't merge yet
git push -u origin main            # first push; sets upstream tracking
```
**Fetch vs pull**: `fetch` downloads without merging, so you can review first. `pull` = `fetch` + `merge` in one step.

## Key Concepts
- **Distributed** — every clone has the full history, not just a snapshot
- **Commit** — an immutable snapshot, identified by a hash
- **HEAD** — pointer to your current position (usually the branch tip)

## Related Concepts
- [[Git and GitHub]] — overview
- [[Git Branching]]
- [[Git Undoing Changes]]
