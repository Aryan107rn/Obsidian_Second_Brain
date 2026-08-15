---
tags: [git, github, version-control, command-line, computer-science, devops]
aliases: [Git Commands, GitHub Basics]
created: 2026-08-08
updated: 2026-08-14
---

# Git & GitHub — Architecture & Command Reference

**Git** is a distributed version control system that tracks changes to files over time. **GitHub** is a cloud hosting service for Git repositories, adding collaboration features (pull requests, issues, actions) on top of Git.

---

## 🖼️ Git 4-Tier Architecture & Workflow

![[git-workflow-architecture.svg]]

---

## The Core Workflow

Changes move through four stages:
1. **Working directory** — your actual edited files on disk
2. **Staging area (Index)** — files marked with `git add` to be included in the next snapshot
3. **Local repo (.git)** — committed snapshots (history), stored on your machine
4. **Remote (GitHub)** — cloud-hosted repository, synchronized via `push` / `pull`

---

## Key Concepts

- **Distributed**: Every clone has the full history — not just a snapshot from a central server.
- **Commit**: An immutable snapshot of the repository at a point in time, identified by a unique SHA-1/SHA-256 hash.
- **HEAD**: A reference pointer to your current position (usually the tip of the current active branch).

---

## Command Reference

### Setup & Config
```bash
git init                             # Start a new local repository
git clone <url>                      # Clone a remote repo
git config --global user.name "You"
git config --global user.email "you@example.com"
```

### Daily Workflow
```bash
git status                # Inspect modified & staged files
git add <file>            # Stage a specific file
git add .                  # Stage all changes in current dir
git commit -m "message"    # Commit staged snapshot
git push origin <branch>   # Push commits to remote
git pull origin <branch>   # Fetch + merge from remote
```

### Branching & Merging
```bash
git branch                 # List branches
git branch <name>          # Create a new branch
git switch <name>          # Switch to a branch (or git checkout <name>)
git switch -c <name>       # Create and switch in one step
git merge <name>           # Merge target branch into current branch
git branch -d <name>       # Safely delete merged branch
```

### Inspecting History
```bash
git log                        # Full commit history
git log --oneline --graph       # Compact visual ASCII history
git diff                        # Unstaged changes vs. staging area
git diff --staged               # Staged changes vs. last commit
git show <commit_hash>          # Details of a specific commit
```

### Undoing Changes Safely

| Command | Effect | Scope |
| :--- | :--- | :--- |
| `git restore <file>` | Discard uncommitted edits to a file | Working directory |
| `git restore --staged <file>`| Unstage file (preserves code edits) | Staging Area |
| `git reset --soft HEAD~1` | Undo last commit, keep changes staged | Local repo $\to$ Staging |
| `git reset --hard HEAD~1` | **Destructive**: Erase last commit and all edits | All local stages |
| `git revert <commit_hash>`| Safe: Creates a new commit undoing previous commit | Shared branches |

---

## 🔗 Related Concepts
- [[Git Rebase]] — Linear commit history alternative to merging
- [[Git Stash]] — Temporarily stashing dirty uncommitted work
- [[Git Tags]] — Marking release versions
- [[Merge Conflicts]] — How to resolve colliding changes
- [[Linux]] — The shell environment where Git commands run
