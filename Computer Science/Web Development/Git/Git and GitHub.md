---
tags: [git, github, version-control, moc, computer-science]
aliases: [Git Commands, Git MOC]
created: 2026-08-08
updated: 2026-08-16
---

# Git & GitHub — MOC

**Git** is a distributed version control system that tracks changes to files over time. **GitHub** is a cloud hosting service for Git repositories, adding collaboration features (pull requests, issues, actions) on top of Git.

## The Core Workflow

Changes move through four stages:

```
Working dir  --git add-->  Staging area  --git commit-->  Local repo  --git push-->  Remote (GitHub)
```
- **Working directory** — your actual edited files
- **Staging area (Index)** — files marked with `git add` for the next commit
- **Local repo (`.git`)** — committed snapshots (history), stored on your machine
- **Remote** — the hosted copy (GitHub), synced via `push`/`pull`

## Key Concepts
- **Distributed** — every clone has the full history, not just a snapshot from a central server
- **Commit** — an immutable snapshot of the repo, identified by a hash
- **HEAD** — pointer to your current position (usually the tip of the active branch)

## Notes in this folder

| Note | Covers |
|---|---|
| [[Git Basics]] | Setup, daily add/commit/push/pull flow, viewing history |
| [[Git Branching]] | Creating, switching, merging, deleting branches |
| [[Git Undoing Changes]] | restore / reset / revert — picking the right undo tool |
| [[GitHub Basics]] | Fork, clone, PR, issues, Actions, `.gitignore` |
| [[Git Stash]] | Parking uncommitted work temporarily |
| [[Git Rebase]] | Replaying commits for linear history |
| [[Git Tags]] | Marking release points |
| [[Merge Conflicts]] | Resolving conflicting changes |

## Best Practices
- Commit frequently, one logical change per commit
- Write present-tense messages: `add login form`, not `added login form`
- One feature per branch; pull before you push
- Never commit secrets — if one slips in, rotate it immediately
- Review with `git diff` / `git status` before every commit

## Related Concepts
- [[Linux]] — Git is typically run from the same command-line environment
- [[SSH]] — commonly used to authenticate `git push`/`pull` with GitHub

## Open Questions / To Explore Later
- Git Flow and other branching strategies
- GitHub Actions / CI pipelines in depth
- Interactive rebase (`rebase -i`)
