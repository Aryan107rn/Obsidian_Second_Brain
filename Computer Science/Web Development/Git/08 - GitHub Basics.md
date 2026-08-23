---
tags: [git, github, version-control]
aliases: [GitHub Basics, github concepts, fork vs clone]
created: 2026-08-16
---

# 08 - GitHub Basics

GitHub is a cloud hosting service for Git repositories — it adds collaboration features (pull requests, issues, project boards, Actions/CI) **on top of** plain Git. These concepts are GitHub-specific, not part of Git itself (GitLab/Bitbucket have their own equivalents).

## Core Concepts
- **Fork** — your own copy of someone else's repo, created under your GitHub account. Lets you propose changes to a project you don't have write access to.
- **Clone** — a working copy of a repo downloaded to your local machine. You can clone your own repos, forks, or (if public) anyone's repo.
- **Pull Request (PR)** — a proposal to merge changes from one branch/fork into another, with review, comments, and (usually) required checks before merging.
- **Issues** — a tracker for bugs, tasks, and feature requests, often linked to PRs that resolve them.
- **Actions** — CI/CD automation triggered by events like push or PR (running tests, building, deploying) — defined in `.github/workflows/*.yml`.
- **`.gitignore`** — a file listing patterns Git should never track (e.g. `node_modules/`, `.env`, build output) — prevents accidentally committing generated files or secrets.

## Fork vs Clone

| | Fork | Clone |
|---|---|---|
| Where the copy lives | On GitHub (a new remote repo) | On your local machine |
| Ownership | You own the forked copy | A working copy of an existing repo |
| Typical use | Contributing to projects you don't own | Day-to-day development on repos you have access to |

**Typical open-source contribution flow:** fork the repo on GitHub → clone *your fork* locally → make changes on a branch → push to your fork → open a PR from your fork's branch into the original repo.

## Git in GUI Tools
**GitHub Desktop** and **VS Code's Source Control panel** wrap the exact same Git commands behind buttons — staging is `+`, committing is a message box + checkmark, "sync" combines `push` + `pull`. Same Git underneath, just a visual layer — understanding the command-line workflow makes any GUI tool immediately readable.

## Common mistakes
- Cloning the original repo instead of your fork, then being unable to push (no write access) — always clone *your fork* when contributing to someone else's project.
- Forgetting to keep a fork in sync with the original ("upstream") repo — forks don't auto-update; you need to add the original as a remote (`git remote add upstream ...`) and periodically merge/rebase from it.
- Committing secrets (API keys, `.env` files) because `.gitignore` wasn't set up before the first commit — once committed, the secret is in history forever unless you rewrite history (and even then, treat it as compromised and rotate it).

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Basics]]
