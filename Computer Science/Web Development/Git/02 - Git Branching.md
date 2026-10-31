---
tags: [git, version-control, command-line]
aliases: [Git Branching, git branches, branching]
created: 2026-08-16
updated: 2026-10-10
---

# 02 - Git Branching

## What is a branch?
A **branch** is an independent line of development — technically just a lightweight, movable **pointer to a commit**. Creating one copies nothing; it just adds a new label. That's why branching in Git is nearly instant and cheap.

![[git-feature-branch-workflow.png|860]]

## Why branch instead of committing straight to `main`?
`main` should always be in a working, deployable state. A branch lets you build a feature, fix a bug, or try a risky idea — committing half-finished work in small steps — without destabilising the code everyone else relies on. If the experiment fails, delete the branch; `main` never noticed.

## Commands
```bash
git branch                    # list local branches (* = current)
git branch -a                 # include remote-tracking branches
git branch feature/login      # create (does not switch)
git switch feature/login      # switch to it
git switch -c feature/login   # create + switch in one step
git merge feature/login       # merge it INTO the branch you're currently on
git branch -d feature/login   # delete (safe — refuses if unmerged)
git branch -D feature/login   # force delete, even if unmerged
git branch -m new-name        # rename the current branch
```
`git switch` / `git restore` (Git 2.23+) were split out of the overloaded `git checkout`, which used to both switch branches *and* restore files. `git checkout <branch>` and `git checkout -b <branch>` still work and you'll see them everywhere.

## Merging
```bash
git switch main
git pull                        # make sure main is current first
git merge feature/login
```
Two outcomes:

```
Before:   A──B ← main
              \
               C──D ← feature

After `git merge feature` (on main):
          A──B──C──D ← main, feature   (main's pointer just slides forward)
```
**Fast-forward** — `main` hasn't moved since `feature` branched, so Git simply slides the `main` pointer forward to D. No new commit is created.



```excalidraw
{
  "elements": [
    {
      "id": "bd9e22b0459d4e6d972a",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1710234162,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a0",
      "seed": 1703568647,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 260,
      "y": 244,
      "roundness": {
        "type": 2
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 140,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          140,
          0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "ddf3dbb225374da38f77",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1146837486,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "seed": 400195184,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 680,
      "y": 464,
      "roundness": {
        "type": 2
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 140,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          140,
          0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "c5f27746e3e841bda11f",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1158808562,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a2",
      "seed": 1341037201,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 434,
      "y": 278,
      "roundness": {
        "type": 2
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 1,
      "height": 152,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          32
        ],
        [
          0,
          122
        ],
        [
          0,
          152
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "37777e6c1197462bb6ea",
      "type": "text",
      "version": 2,
      "versionNonce": 1341516846,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a3",
      "seed": 1252863059,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 402,
      "y": 330,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 64,
      "height": 22,
      "text": "branch",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "branch",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25,
      "boundElements": []
    },
    {
      "id": "8b042f8841994d899077",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1137751474,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a4",
      "seed": 1224686914,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 890,
      "y": 464,
      "roundness": {
        "type": 2
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 230,
      "height": 186,
      "points": [
        [
          0,
          0
        ],
        [
          230,
          0
        ],
        [
          230,
          -144
        ],
        [
          174,
          -186
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "041a8101f4e84acb98a8",
      "type": "text",
      "version": 2,
      "versionNonce": 654587502,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a5",
      "seed": 1668337407,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1092,
      "y": 367,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 56,
      "height": 22,
      "text": "merge",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "merge",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25,
      "boundElements": []
    },
    {
      "id": "85ffe526aef64b5b8d3c",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1666396018,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a6",
      "seed": 1205319742,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 680,
      "y": 244,
      "roundness": {
        "type": 2
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 350,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          350,
          0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "7040019748074c21b3b7",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 307181742,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a7",
      "seed": 1758162832,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 190,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "a335acf040c540559eb8",
          "type": "text"
        }
      ]
    },
    {
      "id": "a335acf040c540559eb8",
      "type": "text",
      "version": 2,
      "versionNonce": 1742762290,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a8",
      "seed": 441545686,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 202,
      "y": 220,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "A",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "7040019748074c21b3b7",
      "originalText": "A",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "870b66f3b0924e6795d1",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1501557486,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a9",
      "seed": 195947114,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 400,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "10d52a3a84214a2e87ab",
          "type": "text"
        }
      ]
    },
    {
      "id": "10d52a3a84214a2e87ab",
      "type": "text",
      "version": 2,
      "versionNonce": 1960803058,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aA",
      "seed": 1778604276,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 412,
      "y": 220,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "B",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "870b66f3b0924e6795d1",
      "originalText": "B",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "5e176e2d03b84c859022",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1730110766,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aB",
      "seed": 172294869,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 430,
      "roundness": {
        "type": 3
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "96e47f1340db4583abcb",
          "type": "text"
        }
      ]
    },
    {
      "id": "96e47f1340db4583abcb",
      "type": "text",
      "version": 2,
      "versionNonce": 685198514,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aC",
      "seed": 1623491496,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622,
      "y": 440,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "C",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "5e176e2d03b84c859022",
      "originalText": "C",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "a3dc37db9ee14336ad45",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1821806446,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aD",
      "seed": 1988055290,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 820,
      "y": 430,
      "roundness": {
        "type": 3
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "ea4af7ecc4114549adff",
          "type": "text"
        }
      ]
    },
    {
      "id": "ea4af7ecc4114549adff",
      "type": "text",
      "version": 2,
      "versionNonce": 963569266,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aE",
      "seed": 122305153,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 832,
      "y": 440,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "D",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "a3dc37db9ee14336ad45",
      "originalText": "D",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "2ac8f19ce5634bc790f0",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 103707054,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aF",
      "seed": 294583914,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "58876613137945aeb82e",
          "type": "text"
        }
      ]
    },
    {
      "id": "58876613137945aeb82e",
      "type": "text",
      "version": 2,
      "versionNonce": 258820146,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aG",
      "seed": 946560668,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622,
      "y": 220,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "E",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "2ac8f19ce5634bc790f0",
      "originalText": "E",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "81fb3ade1d854787ac47",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1594887150,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aH",
      "seed": 548072303,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1030,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "d3cce4bcb31a435f8792",
          "type": "text"
        }
      ]
    },
    {
      "id": "d3cce4bcb31a435f8792",
      "type": "text",
      "version": 2,
      "versionNonce": 1656320498,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aI",
      "seed": 259650809,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1042,
      "y": 220,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "M (merge commit)",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "81fb3ade1d854787ac47",
      "originalText": "M (merge commit)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "a37e893476ef4439a4bb",
      "type": "text",
      "version": 2,
      "versionNonce": 1430543918,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aJ",
      "seed": 1503185226,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 45,
      "y": 230,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 130,
      "height": 26,
      "text": "main",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "right",
      "verticalAlign": "middle",
      "originalText": "main",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": [],
      "containerId": null
    },
    {
      "id": "51a3f04ca8f6462a8457",
      "type": "text",
      "version": 2,
      "versionNonce": 1448764338,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aK",
      "seed": 100775997,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 45,
      "y": 450,
      "roundness": null,
      "updated": 1793216928571,
      "link": null,
      "locked": false,
      "width": 130,
      "height": 26,
      "text": "feature",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "right",
      "verticalAlign": "middle",
      "originalText": "feature",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": [],
      "containerId": null
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "theme": "light"
  },
  "files": {},
  "previewHeight": 460,
  "previewZoom": 1
}
```


**Three-way merge** — both branches gained commits, so Git combines them using the common ancestor (B) and records a **merge commit** M with two parents. If both sides changed the same lines, Git stops and asks you to decide — see [[03 - Merge Conflicts|Merge Conflicts]].

`git merge --no-ff feature` forces a merge commit even when a fast-forward is possible (keeps a visible "this was a feature branch" bubble in history). `git merge --squash feature` collapses the branch into one staged change you commit yourself.

## Remote branches and tracking
```bash
git push -u origin feature/login        # publish the branch and set upstream
git branch -vv                          # which local branch tracks which remote branch
git push origin --delete feature/login  # delete the remote branch
git fetch --prune                       # drop local references to branches deleted on the remote
```
`origin/main` is a **remote-tracking branch** — a read-only bookmark of where `main` was on the remote at your last fetch. It moves only when you `fetch`/`pull`.

## Detached HEAD
Normally `HEAD` points at a branch, which points at a commit. Checking out a commit hash, tag, or `origin/main` directly (`git checkout a1b2c3d`) puts you in **detached HEAD**: you're looking at a commit with no branch following your work. New commits made here are orphaned once you switch away. If you want to keep them: `git switch -c rescue-branch` before leaving. (Lost ones can be dug out via [[10 - Git Reflog, Cherry-pick & Recovery|reflog]].)

## Naming conventions
`feature/login-form`, `fix/null-user-crash`, `hotfix/payment-timeout`, `docs/readme-update`. Lowercase, hyphen-separated, prefixed by type; include a ticket/issue number if your team uses one (`feature/123-login-form`). For how teams organise branches overall, see [[09 - Branching Strategies & PR Workflow|Branching Strategies]].

## Common mistakes
- Committing directly on `main` for anything non-trivial — hard to back out cleanly.
- Not checking which branch you're on before committing (`git status` shows it) — commits land on the wrong branch.
- Long-lived branches that drift far from `main` — the longer they live, the nastier the eventual conflicts. Merge/rebase from `main` often.
- Switching branches with uncommitted changes that conflict — Git refuses; commit them or [[05 - Git Stash|stash]] first.
- Deleting a branch with `-D` without checking it was merged — the commits become unreachable (recoverable via reflog for a while, but don't rely on it).

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[01 - Git Basics|Git Basics]]
- [[03 - Merge Conflicts|Merge Conflicts]]
- [[04 - Git Rebase|Git Rebase]] — alternative to merging that produces a linear history
- [[07 - Git Tags|Git Tags]] — fixed points, versus a branch's moving pointer
- [[09 - Branching Strategies & PR Workflow|Branching Strategies & PR Workflow]]
