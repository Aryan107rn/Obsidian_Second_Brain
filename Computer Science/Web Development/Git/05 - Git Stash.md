---
tags: [git, version-control, command-line]
aliases: [Git Stash, git stash]
created: 2026-08-08
updated: 2026-10-10
---

# 05 - Git Stash

## Why it exists
You're mid-feature when an urgent bug lands on `main`. Your changes are half-done, so committing them pollutes history with meaningless "WIP" commits — but Git won't let you switch branches if uncommitted changes would be overwritten. **Stash** shelves your uncommitted work on a stack and gives you a clean working directory; you bring it back later.



```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.26.4",
  "elements": [
    {
      "id": "cd59793bfced4e0389ff",
      "type": "arrow",
      "version": 1,
      "versionNonce": 2109672691,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1319061610,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 310,
      "y": 295,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 190,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          190,
          0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "a4706b2993954604a723",
      "type": "text",
      "version": 1,
      "versionNonce": 1070330920,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 751689860,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 355,
      "y": 258,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 105,
      "height": 24,
      "text": "git stash",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git stash",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "645a85cf94a64f9981cd",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1523860023,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 970806077,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 750,
      "y": 295,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 200,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          200,
          0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "b44cca9eb3c5427e9283",
      "type": "text",
      "version": 1,
      "versionNonce": 2021476149,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1664456871,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 780,
      "y": 258,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 188,
      "height": 24,
      "text": "Working tree is clean",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Working tree is clean",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "3636e6259d2a4905813b",
      "type": "arrow",
      "version": 1,
      "versionNonce": 741690047,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1738709328,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 190,
      "y": 350,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 435,
      "height": 125,
      "points": [
        [
          435,
          0
        ],
        [
          435,
          125
        ],
        [
          0,
          125
        ],
        [
          0,
          0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "ac38355523024a0a9ec1",
      "type": "text",
      "version": 1,
      "versionNonce": 151379013,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 350697095,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 330,
      "y": 438,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 188,
      "height": 24,
      "text": "git stash pop / apply",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git stash pop / apply",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "aa2b30c14b564791899c",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 682363083,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2100841204,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 70,
      "y": 240,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 240,
      "height": 110,
      "boundElements": [
        {
          "id": "6315779e2c394ff290f5",
          "type": "text"
        }
      ]
    },
    {
      "id": "6315779e2c394ff290f5",
      "type": "text",
      "version": 1,
      "versionNonce": 49035283,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 792861538,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 84,
      "y": 252,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 212,
      "height": 86,
      "text": "Uncommitted changes",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "aa2b30c14b564791899c",
      "originalText": "Uncommitted changes",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "2571ce21221b406a8925",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 769707498,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2056283722,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 500,
      "y": 240,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 250,
      "height": 110,
      "boundElements": [
        {
          "id": "83f386ad16bb46f4ad65",
          "type": "text"
        }
      ]
    },
    {
      "id": "83f386ad16bb46f4ad65",
      "type": "text",
      "version": 1,
      "versionNonce": 72304485,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1669680950,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 514,
      "y": 252,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 222,
      "height": 86,
      "text": "Stash stack",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "2571ce21221b406a8925",
      "originalText": "Stash stack",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "d0846ec179e94fa59a5d",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 898265460,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1410413478,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 950,
      "y": 240,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 110,
      "boundElements": [
        {
          "id": "b2b8ea91e3ee4ea2a73c",
          "type": "text"
        }
      ]
    },
    {
      "id": "b2b8ea91e3ee4ea2a73c",
      "type": "text",
      "version": 1,
      "versionNonce": 339644732,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1463709894,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 964,
      "y": 252,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 272,
      "height": 86,
      "text": "Clean tree\nswitch branch, pull, fix bug",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d0846ec179e94fa59a5d",
      "originalText": "Clean tree\nswitch branch, pull, fix bug",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "theme": "light",
    "gridSize": null,
    "zenModeEnabled": false,
    "gridModeEnabled": false
  },
  "files": {},
  "previewHeight": 460,
  "previewZoom": 1,
  "width": 1320,
  "height": 620
}
```



## Core commands
```bash
git stash                              # shelve tracked changes (staged + unstaged)
git stash push -m "half-done login"    # same, with a label (recommended)
git stash -u                           # also stash UNTRACKED files
git stash -a                           # also stash ignored files (rarely needed)
git stash list                         # stash@{0} is the newest
git stash show -p stash@{0}            # preview a stash's diff
git stash apply                        # restore newest, KEEP it in the list
git stash apply stash@{1}              # restore a specific one
git stash pop                          # restore newest AND remove it
git stash branch fix-login stash@{0}   # new branch from the stash's base commit, then apply it
git stash drop stash@{0}               # delete one
git stash clear                        # delete all
```
**`apply` vs `pop`:** `apply` leaves the stash in the list (handy if you want it on several branches, or if applying might conflict). `pop` applies then deletes it — use when you're done with it.

## Real-world example
```bash
git stash push -m "half-done login form"
git switch main
git switch -c hotfix/typo
# fix, commit, push, merge...
git switch feature/login
git stash pop
```

## Good to know
- By default stash saves **tracked** files only; brand-new untracked files stay behind unless you add `-u`.
- Stashes are **local** — they are never pushed to a remote.
- Stashes aren't tied to a branch; you can pop one onto a different branch than it came from (which may conflict if the code differs).
- Stash entries aren't permanent: if you drop one by accident, it can sometimes be recovered via `git fsck --unreachable`, but treat stash as temporary storage, not a backup.

## Common mistakes
- Forgetting stashes exist — `git stash list` quietly accumulates old entries; check it periodically.
- Unlabeled stashes (`git stash` repeatedly) — impossible to tell apart later. Use `-m`.
- Expecting new files to be stashed — use `-u`.
- Making conflicting edits on the same lines before popping — pop can conflict, resolved like a [[03 - Merge Conflicts|merge conflict]] (and the stash stays in the list when pop conflicts, so nothing is lost).
- Using stash as long-term storage — if the work matters, put it on a branch and commit it.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[02 - Git Branching|Git Branching]] — switching branches is the main reason to stash
- [[03 - Merge Conflicts|Merge Conflicts]]
- [[04 - Git Rebase|Git Rebase]] — stash first, since rebase needs a clean tree
- [[06 - Git Undoing Changes|Git Undoing Changes]]
