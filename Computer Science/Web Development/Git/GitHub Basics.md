---
tags: [git, github, version-control]
aliases: [github concepts, fork vs clone]
created: 2026-08-16
---

# GitHub Basics

Concepts specific to GitHub (or GitHub-like hosts), sitting on top of plain Git.

## Core Concepts
- **Fork** — your own copy of someone else's repo, under your GitHub account
- **Clone** — a working copy of a repo downloaded to your machine
- **Pull Request (PR)** — a proposal to merge changes from one branch/fork into another, with review and discussion
- **Issues** — tracker for bugs, tasks, and feature requests
- **Actions** — CI/CD automation triggered by events like push or PR (tests, builds, deploys)
- **`.gitignore`** — lists patterns Git should never track (e.g. `node_modules/`, `.env`)

## Fork vs Clone

| | Fork | Clone |
|---|---|---|
| Where the copy lives | On GitHub | On your machine |
| Ownership | You own the copy | Working copy of an existing repo |
| Typical use | Contributing to projects you don't own | Day-to-day development |

## Git in GUI Tools

**GitHub Desktop** and **VS Code's Source Control panel** wrap the same Git commands behind buttons — staging is `+`, committing is a message box + checkmark, sync combines `push` + `pull`. Same Git underneath, just a visual layer.

## Related Concepts
- [[Git and GitHub]] — overview
- [[Git Basics]]
