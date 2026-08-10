---
tags: [git, version-control, command-line]
aliases: [git tag, versioning]
created: 2026-08-08
---

# Git Tags

**Tags** mark important points in history — usually releases. Unlike branches, tags don't move as new commits are added.

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

- **Lightweight** — a simple pointer to a commit, no metadata: `git tag v1.0.0`
- **Annotated** — recommended for releases; stores author, date, and message: `git tag -a v1.0.0 -m "first stable release"`

## Commands

```
git tag                        # list all tags
git tag -l "v1.*"               # list tags matching a pattern
git tag v0.9.0 a1b2c3d           # tag a specific past commit
git show v1.0.0                 # see tag details

git push origin v1.0.0          # push one tag
git push origin --tags          # push all tags

git tag -d v1.0.0                # delete locally
git push origin --delete v1.0.0  # delete on remote
```

## Related Concepts
- [[Git and GitHub]]
- [[Branching Strategies]]
