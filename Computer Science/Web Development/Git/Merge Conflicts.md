---
tags: [git, version-control, command-line]
aliases: [merge conflict, resolving conflicts]
created: 2026-08-08
---

# Merge Conflicts

A **merge conflict** happens when Git cannot automatically combine two changes to the same lines of a file.

## Why They Happen

- Two people edit the same line on different branches
- You edit a file locally while someone else pushes changes to the same lines
- Merging or rebasing branches with overlapping edits

Both branches changed the same lines after splitting at a common ancestor — Git has no way to pick a winner automatically.

## Resolving a Conflict

```
git merge feature/search
# CONFLICT in filename.py
```

Open the file — Git inserts conflict markers:
```
<<<<<<< HEAD
your version
=======
their version
>>>>>>> feature/search
```

Edit the file to keep the correct code, remove the markers entirely, then:
```
git add filename.py
git commit -m "resolve merge conflict in filename.py"
```

## Useful Commands While Resolving

```
git diff                  # working tree vs staging area
git diff --staged         # staging area vs last commit (HEAD)
git diff filename.py      # see conflict markers in a specific file
git merge --abort         # bail out entirely, return to pre-merge state
```

## Avoiding Conflicts

- Pull before starting work, and pull before pushing
- Keep branches short-lived
- Communicate with teammates working on the same files

## Related Concepts
- [[Git and GitHub]]
- [[Git Rebase]] — rebasing can also surface conflicts, resolved the same way
- [[Git Stash]]
