---
tags: [git, github, workflow, branching, version-control]
aliases: [Branching Strategies, Git Flow, GitHub Flow, Trunk-Based Development, PR Workflow, pull request workflow]
created: 2026-10-10
---

# 09 - Branching Strategies & PR Workflow

## What is a branching strategy?
Git lets you branch any way you like, so teams need an **agreed convention**: which branches exist, what each is for, how work flows into `main`, and how releases happen. A strategy prevents chaos (everyone pushing to `main`), reduces [[03 - Merge Conflicts|conflicts]], and makes releases predictable.

## The three common strategies

### 1. GitHub Flow — simple, continuous delivery (best default)


```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
  "elements": [
    {
      "id": "9bd1f8b0b4d84189b37f",
      "type": "arrow",
      "version": 1,
      "versionNonce": 792590832,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 278488363,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 260,
      "y": 244,
      "roundness": {
        "type": 2
      },
      "updated": 1,
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
      "endArrowhead": "arrow"
    },
    {
      "id": "3ee5b8865a484dd8bfb7",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1452114046,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 436837543,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 680,
      "y": 464,
      "roundness": {
        "type": 2
      },
      "updated": 1,
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
      "endArrowhead": "arrow"
    },
    {
      "id": "11a64d2f8e3a469d8997",
      "type": "arrow",
      "version": 1,
      "versionNonce": 555482007,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1177394271,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 434,
      "y": 278,
      "roundness": {
        "type": 2
      },
      "updated": 1,
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
      "endArrowhead": "arrow"
    },
    {
      "id": "8ebe4f40046740de9586",
      "type": "text",
      "version": 1,
      "versionNonce": 1247327279,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1081605410,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 402.0,
      "y": 330.0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 64,
      "height": 22,
      "text": "branch",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "branch",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "a29f34a9bc8e49c3b772",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1224434610,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 341725606,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 890,
      "y": 278,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 230,
      "height": 186,
      "points": [
        [
          0,
          186
        ],
        [
          230,
          186
        ],
        [
          230,
          42
        ],
        [
          174,
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
      "id": "0819422f781d452f9306",
      "type": "text",
      "version": 1,
      "versionNonce": 624390540,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1552452824,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1092.0,
      "y": 367.0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 56,
      "height": 22,
      "text": "merge",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "merge",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "48df4bfcc4e440c39e61",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1558659959,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 586711263,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 190,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "a2370ec562a84db9af54",
          "type": "text"
        }
      ]
    },
    {
      "id": "a2370ec562a84db9af54",
      "type": "text",
      "version": 1,
      "versionNonce": 1782887317,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 689371663,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 202,
      "y": 220,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "main (deployable)",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "48df4bfcc4e440c39e61",
      "originalText": "main (deployable)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "e1b6a68efdc7437896d1",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 210467639,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 856642526,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 400,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "535dbb040dce4b9bb960",
          "type": "text"
        }
      ]
    },
    {
      "id": "535dbb040dce4b9bb960",
      "type": "text",
      "version": 1,
      "versionNonce": 1037200023,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 193285768,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 412,
      "y": 220,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "deploy",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "e1b6a68efdc7437896d1",
      "originalText": "deploy",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "10de852eaccd4342ac48",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 28952339,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1184079698,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 430,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "e90257c1c48044aaa9fe",
          "type": "text"
        }
      ]
    },
    {
      "id": "e90257c1c48044aaa9fe",
      "type": "text",
      "version": 1,
      "versionNonce": 1068396653,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2131293187,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622,
      "y": 440,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "work",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "10de852eaccd4342ac48",
      "originalText": "work",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "9e484b51e5ff442e907c",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 2128961940,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1666656165,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 820,
      "y": 430,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "b815af40ae424bc99dea",
          "type": "text"
        }
      ]
    },
    {
      "id": "b815af40ae424bc99dea",
      "type": "text",
      "version": 1,
      "versionNonce": 1409949456,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 167289233,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 832,
      "y": 440,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "more work",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "9e484b51e5ff442e907c",
      "originalText": "more work",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "e1edf2b66e064fdf8c92",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 243425545,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 903645176,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1030,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "0085c8dab1244f6b9d9d",
          "type": "text"
        }
      ]
    },
    {
      "id": "0085c8dab1244f6b9d9d",
      "type": "text",
      "version": 1,
      "versionNonce": 671990327,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 933534662,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1042,
      "y": 220,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "PR merged",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "e1edf2b66e064fdf8c92",
      "originalText": "PR merged",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "b358ac33cca144688880",
      "type": "text",
      "version": 1,
      "versionNonce": 1288202669,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 472004142,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 45,
      "y": 230,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 130,
      "height": 26,
      "text": "main",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "right",
      "verticalAlign": "middle",
      "originalText": "main",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20
    },
    {
      "id": "57a8e0ab759d418b96ca",
      "type": "text",
      "version": 1,
      "versionNonce": 1993249403,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 900314112,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 45,
      "y": 450,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 130,
      "height": 26,
      "text": "feature/login",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "right",
      "verticalAlign": "middle",
      "originalText": "feature/login",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": null,
    "theme": "light",
    "zenModeEnabled": false,
    "gridModeEnabled": false
  },
  "files": {},
  "width": 1088,
  "height": 650,
  "previewHeight": 460,
  "previewZoom": 1
}
```


Rules:
1. `main` is **always deployable**.
2. Branch off `main` for every change, with a descriptive name.
3. Commit and push regularly.
4. Open a **Pull Request**; get review + passing CI.
5. Merge into `main`, then deploy (often automatically).
6. Delete the branch.

Best for: web apps, small/medium teams, continuous deployment. Minimal ceremony — only `main` plus short-lived feature branches.

### 2. Git Flow — structured, scheduled releases
Long-lived branches:
- `main` — production code only (each commit a release, [[07 - Git Tags|tagged]]).
- `develop` — integration branch for the next release.
Supporting branches:
- `feature/*` — branch from `develop`, merge back to `develop`.
- `release/*` — stabilisation before a release; merges into `main` and `develop`.
- `hotfix/*` — urgent production fixes; branch from `main`, merge into `main` and `develop`.

Best for: software with explicit versioned releases and multiple supported versions (installed apps, libraries, embedded). **Trade-off:** more branches, more merging, slower flow; its author later noted it's a poor fit for continuously deployed web apps.

### 3. Trunk-Based Development — fastest integration
Everyone integrates into `main` (the "trunk") at least daily, via very short-lived branches (hours to a day or two) or directly. Unfinished features are hidden behind **feature flags** so incomplete code can ship safely switched off. Requires strong automated testing and CI.

Best for: mature teams with good CI/CD wanting minimal merge pain.

| | GitHub Flow | Git Flow | Trunk-Based |
|---|---|---|---|
| Long-lived branches | `main` | `main` + `develop` | `main` |
| Branch lifetime | days | days–weeks | hours–1 day |
| Release style | continuous | scheduled | continuous |
| Complexity | low | high | low (needs CI discipline) |
| Typical fit | most web projects | versioned products | high-performing CI/CD teams |

For learning projects and most freelance/web work, **GitHub Flow** is the right choice.

## The Pull Request workflow, end to end


```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
  "elements": [
    {
      "id": "f53c0c0fc3414bccaa78",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1139739332,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 524403716,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 290,
      "y": 215,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 70,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          70,
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
      "id": "0018b53972f1485f85e0",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1351521884,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1381829719,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 580,
      "y": 215,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 70,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          70,
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
      "id": "cc90afa3e1d44298ace0",
      "type": "arrow",
      "version": 1,
      "versionNonce": 2104138098,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1274292730,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 870,
      "y": 215,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 80,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          80,
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
      "id": "8c3db0e6fa2f42b481ee",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1393969166,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1242754066,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1075,
      "y": 305,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 1,
      "height": 75,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          75
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "803e3cd1130e4044bd6e",
      "type": "arrow",
      "version": 1,
      "versionNonce": 906369146,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 29593528,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 615,
      "y": 445,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 335,
      "height": 185,
      "points": [
        [
          335,
          0
        ],
        [
          255,
          0
        ],
        [
          255,
          130
        ],
        [
          0,
          130
        ],
        [
          0,
          185
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "a402d68958e7433d84f6",
      "type": "text",
      "version": 1,
      "versionNonce": 1371000955,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1568096250,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 700,
      "y": 542,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 180,
      "height": 24,
      "text": "No — request changes",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "No — request changes",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "e09feb8cf10d4507a28b",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1511774820,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 893012092,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1210,
      "y": 445,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 120,
      "height": 185,
      "points": [
        [
          0,
          0
        ],
        [
          120,
          0
        ],
        [
          120,
          185
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "d446047d9fbb493782de",
      "type": "text",
      "version": 1,
      "versionNonce": 765893948,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 156178272,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1220,
      "y": 405,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 132,
      "height": 24,
      "text": "Yes — approved",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Yes — approved",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "110af0513cec4823bd76",
      "type": "arrow",
      "version": 1,
      "versionNonce": 359765340,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2006636202,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 400,
      "y": 260,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 360,
      "height": 415,
      "points": [
        [
          80,
          415
        ],
        [
          0,
          415
        ],
        [
          0,
          55
        ],
        [
          360,
          55
        ],
        [
          360,
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
      "id": "5a1504543cee4778ba2e",
      "type": "text",
      "version": 1,
      "versionNonce": 1997198616,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1512246132,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 440,
      "y": 605,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 236,
      "height": 24,
      "text": "Push fixes and review again",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Push fixes and review again",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "825107cbdc0340e09766",
      "type": "arrow",
      "version": 1,
      "versionNonce": 2113488596,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1982080187,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1385,
      "y": 720,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 1,
      "height": 70,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          70
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "09b8efc6b10146a1a4c3",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1341209377,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 117748093,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 70,
      "y": 170,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 90,
      "boundElements": [
        {
          "id": "7ae90abfb96b46208884",
          "type": "text"
        }
      ]
    },
    {
      "id": "7ae90abfb96b46208884",
      "type": "text",
      "version": 1,
      "versionNonce": 1850050180,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1957900206,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 84,
      "y": 182,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 66,
      "text": "Create feature branch",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "09b8efc6b10146a1a4c3",
      "originalText": "Create feature branch",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "0f6347f4bbf54a179914",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1721020219,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1620486260,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 360,
      "y": 170,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 90,
      "boundElements": [
        {
          "id": "f66b74843bf146359e90",
          "type": "text"
        }
      ]
    },
    {
      "id": "f66b74843bf146359e90",
      "type": "text",
      "version": 1,
      "versionNonce": 1439915652,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1406155552,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 374,
      "y": 182,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 66,
      "text": "Commit + push",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "0f6347f4bbf54a179914",
      "originalText": "Commit + push",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "48412d640cb345f4a5b7",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 270780774,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1981706764,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 650,
      "y": 170,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 90,
      "boundElements": [
        {
          "id": "0c13fffd413f4cbd94c4",
          "type": "text"
        }
      ]
    },
    {
      "id": "0c13fffd413f4cbd94c4",
      "type": "text",
      "version": 1,
      "versionNonce": 1961951937,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1628495684,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 664,
      "y": 182,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 66,
      "text": "Open pull request",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "48412d640cb345f4a5b7",
      "originalText": "Open pull request",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "5fc6517061ef4328a32b",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1990906391,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1756907283,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 950,
      "y": 170,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 250,
      "height": 135,
      "boundElements": [
        {
          "id": "160bed4c4edb488ba947",
          "type": "text"
        }
      ]
    },
    {
      "id": "160bed4c4edb488ba947",
      "type": "text",
      "version": 1,
      "versionNonce": 1599829798,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 120618237,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 964,
      "y": 182,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 222,
      "height": 111,
      "text": "CI checks + review",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "5fc6517061ef4328a32b",
      "originalText": "CI checks + review",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "477f93468fea46899fb4",
      "type": "diamond",
      "version": 1,
      "versionNonce": 569651413,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 927045945,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 950,
      "y": 380,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 260,
      "height": 130,
      "boundElements": [
        {
          "id": "fa08e6d1c5e7490d8222",
          "type": "text"
        }
      ]
    },
    {
      "id": "fa08e6d1c5e7490d8222",
      "type": "text",
      "version": 1,
      "versionNonce": 894598640,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1646953397,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 964,
      "y": 392,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 232,
      "height": 106,
      "text": "Approved and checks pass?",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "477f93468fea46899fb4",
      "originalText": "Approved and checks pass?",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "37b09bb1a1b34663a1fb",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 195676665,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 638306659,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 480,
      "y": 630,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 270,
      "height": 90,
      "boundElements": [
        {
          "id": "d46c78a6bd964d7ca0fa",
          "type": "text"
        }
      ]
    },
    {
      "id": "d46c78a6bd964d7ca0fa",
      "type": "text",
      "version": 1,
      "versionNonce": 813177431,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 834554504,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 494,
      "y": 642,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 242,
      "height": 66,
      "text": "Update branch with fixes",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "37b09bb1a1b34663a1fb",
      "originalText": "Update branch with fixes",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "27de9128c1604587881c",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1111801149,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1284028688,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1260,
      "y": 630,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 250,
      "height": 90,
      "boundElements": [
        {
          "id": "49ec46a99e4f4804bf0f",
          "type": "text"
        }
      ]
    },
    {
      "id": "49ec46a99e4f4804bf0f",
      "type": "text",
      "version": 1,
      "versionNonce": 717510874,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 546172741,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1274,
      "y": 642,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 222,
      "height": 66,
      "text": "Merge pull request",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "27de9128c1604587881c",
      "originalText": "Merge pull request",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21
    },
    {
      "id": "e56d4aea4e9c4d7386bd",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 56988680,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 42007471,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1245,
      "y": 790,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 280,
      "height": 90,
      "boundElements": [
        {
          "id": "89bf5b972a7a4ce48a39",
          "type": "text"
        }
      ]
    },
    {
      "id": "89bf5b972a7a4ce48a39",
      "type": "text",
      "version": 1,
      "versionNonce": 1878354954,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 637773147,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1259,
      "y": 802,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 252,
      "height": 66,
      "text": "Delete branch; update main",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "e56d4aea4e9c4d7386bd",
      "originalText": "Delete branch; update main",
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
  "width": 1660,
  "height": 940,
  "previewHeight": 460,
  "previewZoom": 1
}
```


Practical habits:
- Branch from an up-to-date `main`: `git switch main && git pull && git switch -c feature/x`.
- Keep PRs small; review quality drops fast with size.
- Before merging, update from `main` (merge or [[04 - Git Rebase|rebase]]) and resolve conflicts on your branch, not on `main`.
- After merging: `git switch main && git pull && git branch -d feature/x`.
- Pick the merge style per team convention (see merge options in [[08 - GitHub Basics|GitHub Basics]]); **squash merge** is popular because it yields one clean commit per PR.

## Common mistakes
- Letting feature branches live for weeks — big, painful merges.
- Pushing directly to `main` on a shared repo (no review, no CI).
- Adopting Git Flow for a small continuously-deployed app — needless overhead.
- Giant PRs mixing refactors, features, and formatting.
- Not deleting merged branches — clutter makes the branch list unreadable.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]]
- [[02 - Git Branching|Git Branching]] — the mechanics these strategies use
- [[03 - Merge Conflicts|Merge Conflicts]]
- [[04 - Git Rebase|Git Rebase]]
- [[07 - Git Tags|Git Tags]] — release marking in Git Flow
- [[08 - GitHub Basics|GitHub Basics]] — PRs, branch protection, Actions
