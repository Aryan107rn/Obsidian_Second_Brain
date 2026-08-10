---
tags: [git, github, version-control, command-line, computer-science, devops]
aliases: [Git Commands, GitHub Basics]
created: 2026-08-08
updated: 2026-08-09
---

# Git & GitHub

**Git** is a distributed version control system that tracks changes to files over time. **GitHub** is a cloud hosting service for Git repositories, adding collaboration features (pull requests, issues, actions) on top of Git.

## The Core Workflow

Changes move through four stages:

```
Working dir  --git add-->  Staging area  --git commit-->  Local repo  --git push-->  Remote (GitHub)
```
- **Working directory** — your actual edited files
- **Staging area** — files marked to be included in the next commit
- **Local repo** — committed snapshots (history), stored on your machine
- **Remote** — a hosted copy (GitHub/GitLab), synced via `push`/`pull`

## Key Concepts

- **Distributed**: every clone has the full history — not just a snapshot from a central server.
- **Commit**: an immutable snapshot of the repo at a point in time, identified by a hash.
- **HEAD**: pointer to your current position (usually the tip of the current branch).

## Command Reference

### Setup
```
git init                             # start a new repo
git clone url                        # copy an existing remote repo
git config --global user.name "You"
git config --global user.email "you@x.com"
```

### Daily Flow
```
git status                # see what changed
git add file               # stage a specific file
git add .                  # stage everything
git commit -m "message"    # save a snapshot
git push                   # send commits to remote
git pull                   # fetch + merge from remote
```

### Branching
Branches let you work on features in isolation without affecting `main`.
```
git branch                 # list branches
git branch name             # create a branch
git checkout name           # switch to a branch
git checkout -b name        # create and switch in one step
git merge name               # merge a branch into current branch
git branch -d name           # delete a branch
```

### Inspecting History
```
git log                        # full commit history
git log --oneline --graph       # compact visual history
git diff                        # unstaged changes
git show commit_hash             # details of one commit
```

### Remote / GitHub
```
git remote add origin url
git fetch                         # download remote changes without merging
git push -u origin main            # first push; sets upstream tracking
```
**Fetch vs pull**: `git fetch` downloads remote changes without merging them, so you can review before combining. `git pull` is `fetch` + `merge` in one step. Use `fetch` when you want to inspect incoming changes first; `pull` when you trust them.

### Undoing Changes
Git offers different undo tools depending on how far a change has gone:

```
git restore file                  # discard unstaged edits to one file
git restore .                     # discard all unstaged edits
git restore --staged file         # unstage a file (keeps edits)
git reset --soft HEAD~1           # undo last commit, keep changes staged
git reset --hard HEAD~1           # undo last commit, discard changes entirely
git revert HEAD                   # new commit that undoes the last one — safe once pushed
```

**Rule of thumb**: use `reset --hard` only on commits nobody else has pulled. Once a commit is pushed and shared, use `git revert` instead — it adds a new commit undoing the change rather than rewriting history, so it's safe on shared branches.

## GitHub-Specific Concepts

- **Fork** — your own copy of someone else's repository, under your account, hosted on GitHub
- **Clone** — a working copy of a repo downloaded to your machine (can be of your own repo, or of a fork)
- **Pull Request (PR)** — a proposal to merge changes from one branch/fork into another, with review and discussion
- **Issues** — tracker for bugs, tasks, and feature requests
- **Actions** — CI/CD automation that runs on events like push or PR (tests, builds, deploys)
- **`.gitignore`** — a file listing patterns Git should never track (e.g. `node_modules/`, `.env`)

**Fork vs Clone:**

| | Fork | Clone |
|---|---|---|
| Where the copy lives | On GitHub | On your machine |
| Ownership | You own the copy | Working copy of an existing repo |
| Typical use | Contributing to projects you don't own | Day-to-day development |

## Git in GUI Tools

Both **GitHub Desktop** and **VS Code's Source Control panel** wrap the same Git commands behind buttons — staging is `+`, committing is a message box + checkmark, sync is `push`+`pull` combined. Useful as a visual starting point, but it's the same Git underneath.

## Best Practices

- Commit frequently, one logical change per commit
- Write present-tense messages: `add login form`, not `added login form`
- One feature per branch
- Pull before you push
- Never commit secrets — if one slips in, rotate it immediately; removing it from Git doesn't undo the exposure
- Review with `git diff` / `git status` before every commit

## Related Concepts
- [[Linux]] — Git is typically run from the same command-line environment
- [[SSH]] — commonly used to authenticate `git push`/`pull` with GitHub
- [[CI/CD]] — GitHub Actions builds on this concept
- [[Merge Conflicts]] — what happens when two changes collide, and how to resolve them
- [[Git Rebase]] — linear history alternative to merging
- [[Git Stash]] — parking uncommitted work temporarily
- [[Git Tags]] — marking releases permanently
- [[Branching Strategies]] — Git Flow, trunk-based development

## Open Questions / To Explore Later
- Git Flow and other branching strategies in depth
- GitHub Actions / CI pipelines in depth
- Interactive rebase (`rebase -i`) for squashing/editing commits
