---
tags: [git, version-control, command-line]
aliases: [Merge Conflicts, merge conflict, resolving conflicts]
created: 2026-08-08
---

# 03 - Merge Conflicts

## What is a merge conflict?
A **merge conflict** happens when Git cannot automatically combine two changes to the same lines of a file — it needs a human to pick the correct result.

## Why they happen
- Two people edit the **same line** on different branches.
- You edit a file locally while someone else pushes changes to the same lines.
- Merging or rebasing branches with overlapping edits.

The root cause is always the same: both branches changed the same lines after splitting from a common ancestor commit, so Git has no principled way to pick a winner automatically — it has to ask you.

## Resolving a Conflict
```
git merge feature/search

# CONFLICT in filename.py
```

Git inserts conflict markers directly into the file:
```
<<<<<<< HEAD
your version
=======
their version
>>>>>>> feature/search
```
- Everything between `<<<<<<< HEAD` and `=======` is **your current branch's** version.
- Everything between `=======` and `>>>>>>> feature/search` is the **incoming branch's** version.

Edit the file to keep the correct code (which might be one side, the other, or a hand-merged combination of both), **remove the markers entirely**, then:
```
git add filename.py
git commit -m "resolve merge conflict in filename.py"
```

## Useful Commands While Resolving
```
git diff                   # working tree vs staging area
git diff --staged          # staging area vs last commit (HEAD)
git diff filename.py       # see conflict markers in a specific file
git merge --abort          # bail out entirely, return to pre-merge state
```

## Avoiding Conflicts
- Pull before starting work, and pull again before pushing.
- Keep branches short-lived — merge back to `main` frequently.
- Communicate with teammates working on the same files.

## Common mistakes
- Forgetting to remove the conflict markers themselves (`<<<<<<<`, `=======`, `>>>>>>>`) before committing — this silently commits broken, unparseable code.
- Blindly keeping "your version" or "their version" without actually reading both — often the correct resolution needs pieces of both.
- Not running the code/tests after resolving — a syntactically valid merge can still be logically wrong.

## Related Concepts
- [[Git & GitHub MOC]] — overview and full topic index
- [[Git Branching]]
- [[Git Rebase]] — rebasing can also surface conflicts, resolved the same way
- [[Git Stash]]
