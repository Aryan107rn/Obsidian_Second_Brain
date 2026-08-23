---
tags: [git, version-control, command-line]
aliases: [Git Tags, git tag, versioning]
created: 2026-08-08
---

# 07 - Git Tags

## What are tags?
**Tags** mark important, fixed points in history — almost always releases (`v1.0.0`, `v2.1.3`). Unlike branches, a tag doesn't move as new commits are added; it points at one specific commit forever.

```
A─B─C─D
    ↑   ↑
 v1.0.0 main

# after more commits on main:

A─B─C─D─E─F
    ↑       ↑
 v1.0.0   main
```
`v1.0.0` stays pinned to commit C forever; `main` keeps advancing.

## Lightweight vs Annotated
- **Lightweight** — a simple named pointer to a commit, no extra metadata: `git tag v1.0.0`. Roughly equivalent to a branch that never moves.
- **Annotated** — recommended for actual releases; stores author, date, and a message, and is itself a full Git object (not just a pointer): `git tag -a v1.0.0 -m "first stable release"`.

**Rule of thumb:** use annotated tags for anything you'll actually reference later (releases); lightweight tags are fine for quick, throwaway markers.

## Commands
```
git tag                         # list all tags
git tag -l "v1.*"                # list tags matching a pattern
git tag v0.9.0 a1b2c3d            # tag a specific past commit (not just HEAD)
git show v1.0.0                  # see tag details
git push origin v1.0.0           # push one tag
git push origin --tags           # push all tags
git tag -d v1.0.0                 # delete locally
git push origin --delete v1.0.0   # delete on remote
```

**Important:** tags are **not pushed automatically** by a plain `git push` — you must explicitly push them (`--tags` or by name). This trips people up when a release tag exists locally but never made it to GitHub.

## Common mistakes
- Forgetting tags don't push automatically — assuming `git push` sent your new release tag along with your commits.
- Using lightweight tags for releases where the metadata (who tagged it, when, why) would actually matter later.
- Re-tagging the same version number after fixing something — tags are meant to be immutable markers; if you must, delete the old tag both locally and remotely first, and communicate clearly since anyone who already fetched the old tag has a stale reference.

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Branching]] — tags vs branches: fixed point vs moving pointer
