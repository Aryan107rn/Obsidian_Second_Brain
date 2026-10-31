---
tags: [git, version-control, command-line]
aliases: [Merge Conflicts, merge conflict, resolving conflicts]
created: 2026-08-08
updated: 2026-10-10
---

# 03 - Merge Conflicts

## What is a merge conflict?
A **merge conflict** happens when Git cannot automatically combine two sets of changes — usually because both branches edited the **same lines** of the same file (or one deleted a file the other edited). Git stops mid-merge and asks a human to decide.
## Why they happen


```excalidraw
{
  "elements": [
    {
      "id": "68c06df08abd4b30add5",
      "type": "arrow",
      "version": 2,
      "versionNonce": 158914422,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a0",
      "seed": 692489686,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 760,
      "y": 155,
      "roundness": {
        "type": 2
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 520,
      "height": 165,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          65
        ],
        [
          -520,
          65
        ],
        [
          -520,
          165
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
      "id": "b67ea29477724b6bb2eb",
      "type": "arrow",
      "version": 2,
      "versionNonce": 146473706,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "seed": 783499685,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 760,
      "y": 155,
      "roundness": {
        "type": 2
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 520,
      "height": 165,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          65
        ],
        [
          520,
          65
        ],
        [
          520,
          165
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
      "id": "5294fbce884b43f6b81b",
      "type": "arrow",
      "version": 2,
      "versionNonce": 916093110,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a2",
      "seed": 692246378,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 240,
      "y": 410,
      "roundness": {
        "type": 2
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 440,
      "height": 90,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          45
        ],
        [
          440,
          45
        ],
        [
          440,
          90
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
      "id": "a7d0f4ed9fd64dfaa8de",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1894716842,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a3",
      "seed": 2119125339,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1280,
      "y": 410,
      "roundness": {
        "type": 2
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 440,
      "height": 90,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          45
        ],
        [
          -440,
          45
        ],
        [
          -440,
          90
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
      "id": "01f44a7e0da34c31a860",
      "type": "arrow",
      "version": 2,
      "versionNonce": 695595510,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a4",
      "seed": 1168473400,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 680,
      "y": 630,
      "roundness": {
        "type": 2
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 360,
      "height": 140,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          60
        ],
        [
          -360,
          60
        ],
        [
          -360,
          140
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
      "id": "653e47e7024c4713b1ba",
      "type": "text",
      "version": 2,
      "versionNonce": 1184026730,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a5",
      "seed": 1393060977,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 410,
      "y": 660,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 140,
      "height": 24,
      "text": "Different lines",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Different lines",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "2c2717f3c3ac47c485c8",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1024475958,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a6",
      "seed": 987549966,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 840,
      "y": 630,
      "roundness": {
        "type": 2
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 380,
      "height": 140,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          60
        ],
        [
          380,
          60
        ],
        [
          380,
          140
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
      "id": "eaf308fde91344dd8800",
      "type": "text",
      "version": 2,
      "versionNonce": 225522474,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a7",
      "seed": 1968434324,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 900,
      "y": 660,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 244,
      "height": 24,
      "text": "Same line, different content",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Same line, different content",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "2df59a0543e14b50b82a",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 600730742,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a8",
      "seed": 887717771,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 65,
      "roundness": {
        "type": 3
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 90,
      "boundElements": [
        {
          "id": "5f171582296640b395fb",
          "type": "text"
        }
      ]
    },
    {
      "id": "5f171582296640b395fb",
      "type": "text",
      "version": 2,
      "versionNonce": 401948138,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a9",
      "seed": 314921161,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 624,
      "y": 77,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 272,
      "height": 66,
      "text": "Common ancestor commit",
      "fontSize": 24,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "2df59a0543e14b50b82a",
      "originalText": "Common ancestor commit",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": []
    },
    {
      "id": "d32b788b3b1e4d06b20a",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 2071293366,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aA",
      "seed": 1076533713,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 90,
      "y": 320,
      "roundness": {
        "type": 3
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 90,
      "boundElements": [
        {
          "id": "2de2e5571ee64c3ba345",
          "type": "text"
        }
      ]
    },
    {
      "id": "2de2e5571ee64c3ba345",
      "type": "text",
      "version": 2,
      "versionNonce": 858386602,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aB",
      "seed": 260506613,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 104,
      "y": 332,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 272,
      "height": 66,
      "text": "main: line 5 changed to X",
      "fontSize": 24,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d32b788b3b1e4d06b20a",
      "originalText": "main: line 5 changed to X",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": []
    },
    {
      "id": "34ad526c9be444708668",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1630160630,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aC",
      "seed": 270424247,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1130,
      "y": 320,
      "roundness": {
        "type": 3
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 90,
      "boundElements": [
        {
          "id": "ce8b0febd5f845ea800f",
          "type": "text"
        }
      ]
    },
    {
      "id": "ce8b0febd5f845ea800f",
      "type": "text",
      "version": 2,
      "versionNonce": 1387940714,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aD",
      "seed": 6257543,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1144,
      "y": 332,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 272,
      "height": 66,
      "text": "feature: line 5 changed to Y",
      "fontSize": 24,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "34ad526c9be444708668",
      "originalText": "feature: line 5 changed to Y",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": []
    },
    {
      "id": "f3155ab378a7461ab1ee",
      "type": "diamond",
      "version": 12,
      "versionNonce": 362328965,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aE",
      "seed": 173308092,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 609.21875,
      "y": 495.99493408203125,
      "roundness": null,
      "updated": 1793382266871,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 130,
      "boundElements": [
        {
          "id": "8b4dd02b679048b1af12",
          "type": "text"
        }
      ]
    },
    {
      "id": "8b4dd02b679048b1af12",
      "type": "text",
      "version": 12,
      "versionNonce": 228719333,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aF",
      "seed": 1915182109,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 623.21875,
      "y": 507.99493408203125,
      "roundness": null,
      "updated": 1793382266871,
      "link": null,
      "locked": false,
      "width": 272,
      "height": 106,
      "text": "Merge",
      "fontSize": 24,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "f3155ab378a7461ab1ee",
      "originalText": "Merge",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": []
    },
    {
      "id": "c7a24ac291144632ac90",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1076986230,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aG",
      "seed": 1505991583,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 180,
      "y": 770,
      "roundness": {
        "type": 3
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 280,
      "height": 90,
      "boundElements": [
        {
          "id": "1cdf11d4bbd54ad088a2",
          "type": "text"
        }
      ]
    },
    {
      "id": "1cdf11d4bbd54ad088a2",
      "type": "text",
      "version": 2,
      "versionNonce": 918157546,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aH",
      "seed": 1189342982,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 194,
      "y": 782,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 252,
      "height": 66,
      "text": "Auto-merged",
      "fontSize": 24,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c7a24ac291144632ac90",
      "originalText": "Auto-merged",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": []
    },
    {
      "id": "8b4999318bc9498b8072",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 615375542,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aI",
      "seed": 1135140137,
      "strokeColor": "#334155",
      "backgroundColor": "#fee2e2",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1050,
      "y": 770,
      "roundness": {
        "type": 3
      },
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 340,
      "height": 110,
      "boundElements": [
        {
          "id": "06fb497b587f49dd9e0b",
          "type": "text"
        }
      ]
    },
    {
      "id": "06fb497b587f49dd9e0b",
      "type": "text",
      "version": 2,
      "versionNonce": 798968746,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aJ",
      "seed": 123149354,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1064,
      "y": 782,
      "roundness": null,
      "updated": 1793332773849,
      "link": null,
      "locked": false,
      "width": 312,
      "height": 86,
      "text": "CONFLICT\nGit can't pick a winner",
      "fontSize": 24,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "8b4999318bc9498b8072",
      "originalText": "CONFLICT\nGit can't pick a winner",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": []
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff"
  },
  "files": {},
  "previewHeight": 460,
  "previewZoom": 1
}
```


Root cause is always the same: both sides diverged from a common ancestor and touched overlapping content, so there's no principled automatic answer. Typical situations:
- Two people edit the same line on different branches.
- You edit locally while a teammate pushes changes to the same lines.
- A merge or [[04 - Git Rebase|rebase]] brings together overlapping edits.
- One side renames/deletes a file the other modified.

## Resolving a conflict, step by step
```bash
git merge feature/search
# Auto-merging app.py
# CONFLICT (content): Merge conflict in app.py
# Automatic merge failed; fix conflicts and then commit the result.
```
1. **Find conflicted files:** `git status` lists them under *Unmerged paths* (`git diff --name-only --diff-filter=U` prints just the names).
2. **Open each file.** Git has inserted markers:
```
<<<<<<< HEAD
your version (current branch)
=======
their version (incoming branch)
>>>>>>> feature/search
```
   - Between `<<<<<<< HEAD` and `=======` → the branch you're **on**.
   - Between `=======` and `>>>>>>> feature/search` → the **incoming** branch.
3. **Edit** to the correct final code — one side, the other, or a hand-written blend — and **delete all three marker lines**.
4. **Test** that it still runs/passes.
5. **Mark resolved and finish:**
```bash
git add app.py
git commit              # Git pre-fills a merge message; or: git merge --continue
```

## Shortcuts and tooling
```bash
git checkout --ours   app.py   # take the current branch's whole file
git checkout --theirs app.py   # take the incoming branch's whole file
git mergetool                  # open a configured visual 3-way merge tool
git merge --abort              # give up; return to the pre-merge state
```
VS Code shows "Accept Current / Accept Incoming / Accept Both" buttons above each conflict block — same markers underneath.

**`ours`/`theirs` flip during a rebase.** While rebasing, "ours" is the branch you're rebasing *onto* and "theirs" is *your own commits being replayed*. This reversal confuses almost everyone — when in doubt, read both sides instead of using the shortcut. In a rebase, finish with `git add <file>` then `git rebase --continue` (not `commit`).

## Avoiding conflicts
- Pull/merge from `main` often so your branch never drifts far.
- Keep branches and PRs small and short-lived.
- Agree on who's touching which files; avoid mass reformatting commits that rewrite every line.
- Use a shared formatter/linter config so whitespace changes don't create phantom conflicts.

## Common mistakes
- Committing with the markers (`<<<<<<<`, `=======`, `>>>>>>>`) still in the file — silently ships broken, unparseable code. Search the project for `<<<<<<<` before committing.
- Blindly accepting "mine" or "theirs" without reading both — the right answer usually needs pieces of each.
- Not running the tests after resolving — a merge can be syntactically clean but logically wrong.
- Panicking and deleting the repo — `git merge --abort` (or `git rebase --abort`) safely undoes the whole attempt.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[02 - Git Branching|Git Branching]] — where merges come from
- [[04 - Git Rebase|Git Rebase]] — can raise the same conflicts, once per replayed commit
- [[05 - Git Stash|Git Stash]] — a stash pop can conflict too
- [[09 - Branching Strategies & PR Workflow|Branching Strategies & PR Workflow]] — short-lived branches reduce conflicts
