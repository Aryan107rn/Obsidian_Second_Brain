---
tags: [git, github, version-control, moc, computer-science]
aliases: [Git and GitHub, Git & GitHub MOC, Git Commands, Git MOC]
created: 2026-08-08
updated: 2026-10-10
---

# 00 - Git & GitHub MOC

**Git** is a distributed version control system that tracks changes to files over time. **GitHub** is a cloud hosting service for Git repositories, adding collaboration features (pull requests, issues, Actions) on top of plain Git.

## The core workflow


```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
  "elements": [
    {
      "id": "7c1bc66b35d94e5595f4",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1807351194,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1278142523,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 330,
      "y": 183.0,
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
          0.0
        ],
        [
          80,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "c6c06526900f44dea215",
      "type": "text",
      "version": 1,
      "versionNonce": 999462470,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1050555666,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 334.0,
      "y": 158.0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 72,
      "height": 22,
      "text": "git add",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git add",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "3282c108fc424f139511",
      "type": "arrow",
      "version": 1,
      "versionNonce": 2128556868,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 408833872,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 630,
      "y": 183.0,
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
          0.0
        ],
        [
          80,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "9787c4a2f7d24996902b",
      "type": "text",
      "version": 1,
      "versionNonce": 381875066,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 722668487,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622.0,
      "y": 158.0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 96,
      "height": 22,
      "text": "git commit",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git commit",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "6bb7707da11d44598b27",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1642978977,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1819720833,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 930,
      "y": 183.0,
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
          0.0
        ],
        [
          80,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "abd0a3760fd443c6970c",
      "type": "text",
      "version": 1,
      "versionNonce": 89178137,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 851123076,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 930.0,
      "y": 158.0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 80,
      "height": 22,
      "text": "git push",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git push",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "154f8767c44246d98ff3",
      "type": "arrow",
      "version": 1,
      "versionNonce": 744752554,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 444985593,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 220.0,
      "y": 103,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 900.0,
      "height": 42,
      "points": [
        [
          900.0,
          42
        ],
        [
          900.0,
          0
        ],
        [
          0.0,
          0
        ],
        [
          0.0,
          42
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "07e75f5ec30e4348be37",
      "type": "text",
      "version": 1,
      "versionNonce": 1331280621,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 583979184,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 630.0,
      "y": 78.0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 80,
      "height": 22,
      "text": "git pull",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git pull",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "31f3c660b8be43f1aaf6",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1010146712,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 798397006,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 110,
      "y": 145,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 76,
      "boundElements": [
        {
          "id": "fdd8197f138b4f5796f5",
          "type": "text"
        }
      ]
    },
    {
      "id": "fdd8197f138b4f5796f5",
      "type": "text",
      "version": 1,
      "versionNonce": 1404417455,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 4941672,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 122,
      "y": 155,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 196,
      "height": 56,
      "text": "Working directory",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "31f3c660b8be43f1aaf6",
      "originalText": "Working directory",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "899b9294df2946e38585",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 950684567,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 983039617,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 410,
      "y": 145,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 76,
      "boundElements": [
        {
          "id": "cab72db9cda54c41a505",
          "type": "text"
        }
      ]
    },
    {
      "id": "cab72db9cda54c41a505",
      "type": "text",
      "version": 1,
      "versionNonce": 1686952193,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1376503116,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 422,
      "y": 155,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 196,
      "height": 56,
      "text": "Staging area",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "899b9294df2946e38585",
      "originalText": "Staging area",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "9774097f2f1b454fa52a",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 2099317903,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2108468935,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 710,
      "y": 145,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 76,
      "boundElements": [
        {
          "id": "6541a408f3974d99aff3",
          "type": "text"
        }
      ]
    },
    {
      "id": "6541a408f3974d99aff3",
      "type": "text",
      "version": 1,
      "versionNonce": 847593011,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1643440217,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 722,
      "y": 155,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 196,
      "height": 56,
      "text": "Local repo (.git)",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "9774097f2f1b454fa52a",
      "originalText": "Local repo (.git)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "e74452e43b4b444e8849",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 920740976,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 230399874,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1010,
      "y": 145,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 76,
      "boundElements": [
        {
          "id": "6dcc4c2dacd846fea365",
          "type": "text"
        }
      ]
    },
    {
      "id": "6dcc4c2dacd846fea365",
      "type": "text",
      "version": 1,
      "versionNonce": 703209064,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1768693622,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1022,
      "y": 155,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 196,
      "height": 56,
      "text": "Remote (GitHub)",
      "fontSize": 23,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "e74452e43b4b444e8849",
      "originalText": "Remote (GitHub)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
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
  "width": 670,
  "height": 618,
  "previewHeight": 460,
  "previewZoom": 1
}
```


- **Working directory** — your files as edited on disk.
- **Staging area (index)** — files marked with `git add` for the next commit.
- **Local repo** — committed snapshots (history) on your machine.
- **Remote** — the hosted copy, synced with `push` / `fetch` / `pull`.

## Key concepts
- **Distributed** — every clone has the full history, not just a snapshot.
- **Commit** — an immutable snapshot identified by a hash.
- **Branch** — a movable pointer to a commit; **tag** — a fixed one.
- **HEAD** — pointer to your current position (usually the tip of the active branch).

## Notes in this folder (in learning order)
| # | Note | Covers |
|---|---|---|
| 01 | [[01 - Git Basics\|Git Basics]] | Setup, four areas, daily flow, commit messages, history, remotes, `.gitignore`, internals |
| 02 | [[02 - Git Branching\|Git Branching]] | Branches, switch/merge, fast-forward vs 3-way, remote tracking, detached HEAD |
| 03 | [[03 - Merge Conflicts\|Merge Conflicts]] | Why they happen, resolving step by step, ours/theirs, avoiding them |
| 04 | [[04 - Git Rebase\|Git Rebase]] | Linear history, interactive rebase, golden rule, `--force-with-lease` |
| 05 | [[05 - Git Stash\|Git Stash]] | Shelving uncommitted work, apply vs pop, untracked files |
| 06 | [[06 - Git Undoing Changes\|Git Undoing Changes]] | restore / reset / revert / amend — decision flowchart |
| 07 | [[07 - Git Tags\|Git Tags]] | Release markers, lightweight vs annotated, GitHub Releases |
| 08 | [[08 - GitHub Basics\|GitHub Basics]] | Fork, clone, PRs, merge options, branch protection, Actions |
| 09 | [[09 - Branching Strategies & PR Workflow\|Branching Strategies & PR Workflow]] | GitHub Flow, Git Flow, trunk-based, PR lifecycle |
| 10 | [[10 - Git Reflog, Cherry-pick & Recovery\|Reflog, Cherry-pick & Recovery]] | Recover "lost" commits, copy single commits, bisect |
| 11 | [[11 - SSH Keys & GitHub Authentication\|SSH Keys & GitHub Authentication]] | SSH key setup, HTTPS tokens, security |

## Which note do I need? (quick finder)
| I want to… | Go to |
|---|---|
| Start using Git / commit & push | [[01 - Git Basics\|Git Basics]] |
| Work on a feature without breaking `main` | [[02 - Git Branching\|Branching]] |
| Fix `<<<<<<<` markers | [[03 - Merge Conflicts\|Merge Conflicts]] |
| Tidy my commits before a PR | [[04 - Git Rebase\|Rebase]] |
| Switch branches with unfinished work | [[05 - Git Stash\|Stash]] |
| Undo a mistake | [[06 - Git Undoing Changes\|Undoing Changes]] → [[10 - Git Reflog, Cherry-pick & Recovery\|Reflog]] |
| Mark a release | [[07 - Git Tags\|Tags]] |
| Contribute to open source | [[08 - GitHub Basics\|GitHub Basics]] |
| Choose a team workflow | [[09 - Branching Strategies & PR Workflow\|Strategies]] |
| Stop typing passwords | [[11 - SSH Keys & GitHub Authentication\|SSH Keys]] |

## Best practices
- Commit often; **one logical change per commit**; present-tense messages (`add login form`).
- One feature per branch; pull before you push; keep branches short-lived.
- Review with `git status` and `git diff` before every commit.
- Never commit secrets — if one slips in, **rotate it immediately** (deleting it from history doesn't undo the exposure).
- Never rewrite history that others have pulled (no rebase/`reset --hard`/force-push on shared branches).
- Protect `main` with required reviews and CI checks.

## Related concepts
- [[Linux]] — the command-line environment Git is usually run from
- [[10 - Semantic Versioning & Dependency Management]] — version numbers used with tags
- [[03 - npm, package.json & Node Modules]] — `node_modules` and `.env` belong in `.gitignore`

## Open questions / to explore later
- GitHub Actions / CI pipelines in depth (deployment, secrets, matrices)
- Git hooks (pre-commit linting, Husky)
- Submodules / monorepos
- Signing commits (GPG/SSH signatures)
