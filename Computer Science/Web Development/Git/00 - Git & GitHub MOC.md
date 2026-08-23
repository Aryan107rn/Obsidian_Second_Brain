---
tags: [git, github, version-control, moc, computer-science]
aliases: [Git and GitHub, Git & GitHub MOC, Git Commands, Git MOC]
created: 2026-08-08
updated: 2026-08-23
---

# 00 - Git & GitHub MOC

**Git** is a distributed version control system that tracks changes to files over time. **GitHub** is a cloud hosting service for Git repositories, adding collaboration features (pull requests, issues, Actions) on top of plain Git.

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

## Notes in this folder (in learning order)

| # | Note | Covers |
|---|---|---|
| 01 | [[Git Basics]] | Setup, the four stages, daily add/commit/push/pull flow, viewing history |
| 02 | [[Git Branching]] | Creating, switching, merging, deleting branches |
| 03 | [[Merge Conflicts]] | Why conflicts happen, resolving them, avoiding them |
| 04 | [[Git Rebase]] | Replaying commits for linear history, merge vs rebase, the golden rule |
| 05 | [[Git Stash]] | Parking uncommitted work temporarily |
| 06 | [[Git Undoing Changes]] | restore / reset / revert — picking the right undo tool |
| 07 | [[Git Tags]] | Marking release points, lightweight vs annotated |
| 08 | [[GitHub Basics]] | Fork, clone, PR, issues, Actions, `.gitignore` |

## Best Practices
- Commit frequently, one logical change per commit.
- Write present-tense messages: `add login form`, not `added login form`.
- One feature per branch; pull before you push.
- Never commit secrets — if one slips in, rotate it immediately (removing it from history doesn't undo the exposure).
- Review with `git diff` / `git status` before every commit.

## Related Concepts
- [[Linux]] — Git is typically run from the same command-line environment
- [[SSH]] — commonly used to authenticate `git push`/`pull` with GitHub

## Open Questions / To Explore Later
- Git Flow and other branching strategies
- GitHub Actions / CI pipelines in depth
- Interactive rebase (`rebase -i`)
