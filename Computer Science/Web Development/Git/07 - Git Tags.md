---
tags: [git, version-control, command-line, github]
aliases: [Git Tags, git tag, versioning, releases]
created: 2026-08-08
updated: 2026-10-10
---

# 07 - Git Tags

## What are tags?
**Tags** mark important, fixed points in history — almost always releases (`v1.0.0`, `v2.1.3`). Unlike a branch, a tag **never moves**: it points at one specific commit forever. Think "bookmark for a release", versus a branch being "a lane that keeps advancing".

```
A─B─C─D
    ↑   ↑
 v1.0.0 main

# after more commits on main:

A─B─C─D─E─F
    ↑       ↑
 v1.0.0   main
```
`v1.0.0` stays pinned to commit C; `main` keeps moving.

## Why use tags?
- Mark exactly what code was shipped in a release, so you can check it out, diff against it, or hotfix it later.
- Give humans readable names for commits instead of hashes.
- Drive automation: CI/CD pipelines commonly trigger a build/deploy when a tag like `v*` is pushed.
- Pair with **Semantic Versioning** (`MAJOR.MINOR.PATCH`) — the same scheme explained for npm packages in [[10 - Semantic Versioning & Dependency Management]]. Bump MAJOR for breaking changes, MINOR for backward-compatible features, PATCH for fixes.

## Lightweight vs annotated
- **Lightweight** — just a named pointer to a commit: `git tag v1.0.0`. No metadata.
- **Annotated** — a full Git object storing tagger name, email, date, and a message: `git tag -a v1.0.0 -m "first stable release"`. Can also be signed (`-s`) for verification.

**Rule of thumb:** annotated for real releases (you'll want to know who tagged it, when, and why); lightweight only for quick, private markers.

## Commands
```bash
git tag                            # list all tags
git tag -l "v1.*"                  # list matching a pattern
git tag -a v1.0.0 -m "release"     # annotated tag on the current commit
git tag -a v0.9.0 a1b2c3d -m "..." # tag a specific past commit
git show v1.0.0                    # tag details + the commit it points to
git checkout v1.0.0                # inspect that release (detached HEAD)
git push origin v1.0.0             # push one tag
git push origin --tags             # push all tags
git tag -d v1.0.0                  # delete locally
git push origin --delete v1.0.0    # delete on the remote
```
**Tags are not pushed by a plain `git push`.** You must push them explicitly. This is the classic "my release tag exists locally but never reached GitHub" mistake.

## Tags and GitHub Releases
On GitHub, a **Release** is a page built on top of a tag: release notes, changelog, and downloadable assets (binaries, zips). Pushing a tag then clicking *Draft a new release* (or automating it with GitHub Actions) turns a bare tag into a published release. See [[08 - GitHub Basics|GitHub Basics]].

## Common mistakes
- Assuming `git push` sends tags — it doesn't.
- Lightweight tags for releases where authorship/date/message matter later.
- Re-tagging the same version after a fix. Tags should be immutable; moving one confuses everyone who already fetched it. Cut a new version (`v1.0.1`) instead; if you must delete and recreate, delete locally *and* remotely and tell the team.
- Inconsistent naming (`1.0`, `v1.0.0`, `release-1`) — pick one scheme (usually `vMAJOR.MINOR.PATCH`).
- Tagging before the commit is final (before CI passes), then needing to move the tag.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[02 - Git Branching|Git Branching]] — fixed pointer (tag) vs moving pointer (branch)
- [[08 - GitHub Basics|GitHub Basics]] — Releases
- [[10 - Semantic Versioning & Dependency Management]] — the versioning scheme tags usually follow
