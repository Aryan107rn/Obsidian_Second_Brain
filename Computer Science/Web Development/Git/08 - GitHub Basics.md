---
tags: [git, github, version-control]
aliases: [GitHub Basics, github concepts, fork vs clone, pull requests]
created: 2026-08-16
updated: 2026-10-10
---

# 08 - GitHub Basics

GitHub is a cloud hosting service for Git repositories. It adds collaboration features — pull requests, issues, code review, project boards, Actions (CI/CD) — **on top of** plain Git. Those features are GitHub-specific; Git itself works without them. (GitLab and Bitbucket offer equivalents.)

## Core concepts
- **Repository (repo)** — a project's code plus its full history, hosted on GitHub; may be public or private.
- **Fork** — your own copy of someone else's repo under your account, so you can propose changes without write access to the original.
- **Clone** — a copy of a repo downloaded to your local machine.
- **Pull Request (PR)** — a proposal to merge one branch (or fork) into another, with review, discussion, and automated checks before merging.
- **Issues** — trackers for bugs, tasks, and feature requests; PRs can reference and auto-close them.
- **Actions** — CI/CD automation triggered by events (push, PR, schedule) and defined in `.github/workflows/*.yml`.
- **Releases** — published versions built on [[07 - Git Tags|tags]], with notes and downloadable assets.
- **README.md** — the repo's front page, rendered on GitHub; first thing visitors read.
- **`.gitignore`** — plain Git, not GitHub; see [[01 - Git Basics|Git Basics]].

## Fork vs clone
| | Fork | Clone |
|---|---|---|
| Where the copy lives | On GitHub (new remote repo under your account) | On your local machine |
| Purpose | Get your own writable copy of a repo you don't own | Work on a repo locally |
| Typical use | Open-source contributions | Daily development |

They're complementary: you **fork** on GitHub, then **clone your fork** locally.

## The open-source contribution flow


```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
  "elements": [
    {
      "id": "1ca4d8c72a6546a3ad1b",
      "type": "text",
      "version": 1,
      "versionNonce": 828388785,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 644228842,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": [
        655,
        518
      ],
      "y": 0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 162,
      "height": 24,
      "text": "git fetch upstream",
      "fontSize": 14,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git fetch upstream",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "fc98da04e68047f38a11",
      "type": "arrow",
      "version": 1,
      "versionNonce": 958189847,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 777238461,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 280,
      "y": 380,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 960,
      "height": 170,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          170
        ],
        [
          960,
          170
        ],
        [
          960,
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
      "id": "5c6799d7db8f46aaaf22",
      "type": "text",
      "version": 1,
      "versionNonce": 1480967781,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1085792601,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": [
        900,
        412
      ],
      "y": 0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 85,
      "height": 24,
      "text": "git push",
      "fontSize": 14,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git push",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "37c874c130914c2b9f6e",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1264443951,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 816975864,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 750,
      "y": 350,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 350,
      "height": 95,
      "points": [
        [
          350,
          0
        ],
        [
          350,
          95
        ],
        [
          0,
          95
        ],
        [
          0,
          30
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "62657a03ddf8462cbd3b",
      "type": "text",
      "version": 1,
      "versionNonce": 61029476,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1440056692,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": [
        465,
        111
      ],
      "y": 0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 154,
      "height": 24,
      "text": "Open Pull Request",
      "fontSize": 14,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Open Pull Request",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "34d0bfefb35b431ea090",
      "type": "arrow",
      "version": 1,
      "versionNonce": 650585419,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 710763773,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 280,
      "y": 145,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 475,
      "height": 115,
      "points": [
        [
          475,
          115
        ],
        [
          475,
          0
        ],
        [
          0,
          0
        ],
        [
          0,
          115
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow"
    },
    {
      "id": "95e0fc8595924a37b9bc",
      "type": "text",
      "version": 1,
      "versionNonce": 1022063010,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 613387069,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": [
        955,
        286
      ],
      "y": 0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 90,
      "height": 24,
      "text": "git clone",
      "fontSize": 14,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git clone",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "11cfcfd0289340be9719",
      "type": "arrow",
      "version": 1,
      "versionNonce": 2082909359,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 543626976,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 890,
      "y": 320,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 210,
      "height": 1,
      "points": [
        [
          0,
          0
        ],
        [
          210,
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
      "id": "6d2fbd30574041dc8380",
      "type": "text",
      "version": 1,
      "versionNonce": 170830431,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2054470777,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": [
        492,
        286
      ],
      "y": 0,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 85,
      "height": 24,
      "text": "Fork",
      "fontSize": 14,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Fork",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17
    },
    {
      "id": "4d8ac052c77144818ccd",
      "type": "arrow",
      "version": 1,
      "versionNonce": 309969815,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 953661925,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 420,
      "y": 320,
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
      "id": "5f9ba74a37be45f59d65",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 620438964,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 40976510,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 140,
      "y": 260,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 280,
      "height": 120,
      "boundElements": [
        {
          "id": "3b6d041d9a814d1ca6b5",
          "type": "text"
        }
      ]
    },
    {
      "id": "3b6d041d9a814d1ca6b5",
      "type": "text",
      "version": 1,
      "versionNonce": 137545243,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1014990736,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 158,
      "y": 280,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 244,
      "height": 80,
      "text": "Original repo\n(upstream)",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "5f9ba74a37be45f59d65",
      "originalText": "Original repo\n(upstream)",
      "autoResize": false,
      "lineHeight": 1.3,
      "baseline": 23
    },
    {
      "id": "62e0009feaf841c09506",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1817089510,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 35267432,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 260,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 280,
      "height": 120,
      "boundElements": [
        {
          "id": "f904cf75900049f4a13d",
          "type": "text"
        }
      ]
    },
    {
      "id": "f904cf75900049f4a13d",
      "type": "text",
      "version": 1,
      "versionNonce": 484458665,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1871079055,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 628,
      "y": 280,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 244,
      "height": 80,
      "text": "Your fork\n(origin, on GitHub)",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "62e0009feaf841c09506",
      "originalText": "Your fork\n(origin, on GitHub)",
      "autoResize": false,
      "lineHeight": 1.3,
      "baseline": 23
    },
    {
      "id": "5a9825c371e045138019",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1619594232,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 20549645,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1100,
      "y": 260,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 280,
      "height": 120,
      "boundElements": [
        {
          "id": "ba7a9fc598b246078db9",
          "type": "text"
        }
      ]
    },
    {
      "id": "ba7a9fc598b246078db9",
      "type": "text",
      "version": 1,
      "versionNonce": 508591879,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 187075716,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1118,
      "y": 280,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 244,
      "height": 80,
      "text": "Local copy\nbranch, edit, commit",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "5a9825c371e045138019",
      "originalText": "Local copy\nbranch, edit, commit",
      "autoResize": false,
      "lineHeight": 1.3,
      "baseline": 23
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
  "width": 1500,
  "height": 680,
  "previewHeight": 460,
  "previewZoom": 1
}
```


1. **Fork** the project on GitHub.
2. `git clone <your-fork-url>`.
3. Add the original as a second remote called `upstream`:
```bash
git remote add upstream https://github.com/original-owner/project.git
git remote -v        # origin = your fork, upstream = the original
```
4. Create a branch: `git switch -c fix/typo-in-readme`.
5. Edit, commit, `git push -u origin fix/typo-in-readme`.
6. On GitHub click **Compare & pull request** → target the original repo's `main`.
7. Respond to review feedback by pushing more commits to the same branch; the PR updates automatically.

### Keeping your fork in sync
Forks don't update themselves.
```bash
git fetch upstream
git switch main
git merge upstream/main        # or: git rebase upstream/main
git push origin main
```

## Anatomy of a good pull request
- **Title** that says what changes; **description** that says why, how to test it, and links the issue.
- Small and focused — one concern per PR; big PRs get slow, shallow reviews.
- Auto-close issues by writing `Closes #42`, `Fixes #42`, or `Resolves #42` in the description or a commit message.
- **Draft PR** — open early to get feedback while work is in progress.
- Reviewers can **Comment**, **Approve**, or **Request changes**.

### Merge options on GitHub
| Option | Result | Use when |
|---|---|---|
| **Merge commit** | Keeps all branch commits + adds a merge commit | You want the full history preserved |
| **Squash and merge** | Collapses the PR into **one** commit on `main` | Branch has noisy commits; want a tidy `main` |
| **Rebase and merge** | Replays commits linearly, no merge commit | You want linear history and clean commits |

More on how teams combine these with branching rules: [[09 - Branching Strategies & PR Workflow|Branching Strategies & PR Workflow]].

## Protecting `main`
**Branch protection rules** (or newer *rulesets*) can require: a pull request instead of direct pushes, N approving reviews, passing status checks (tests/CI), and up-to-date branches before merging. Standard on any team repo.

## GitHub Actions (CI) — a minimal example
`.github/workflows/ci.yml`:
```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm test
```
Every push/PR now runs the tests on a fresh machine; the result appears as a ✅/❌ on the PR, which branch protection can require before merging. (`npm ci` installs exactly what `package-lock.json` specifies — see [[10 - Semantic Versioning & Dependency Management]].)

## Authenticating: HTTPS vs SSH
- **HTTPS** remotes (`https://github.com/...`) authenticate with a **Personal Access Token** or a credential helper — GitHub no longer accepts account passwords for Git operations.
- **SSH** remotes (`git@github.com:...`) authenticate with a key pair, so you aren't prompted repeatedly.

Setup steps: [[11 - SSH Keys & GitHub Authentication|SSH Keys & GitHub Authentication]].

## Git in GUI tools
**GitHub Desktop** and **VS Code's Source Control panel** wrap the same Git commands in buttons: `+` stages, the message box + ✔ commits, "Sync" does `pull` + `push`. Understanding the command-line flow makes any GUI instantly readable.

## Common mistakes
- Cloning the original repo instead of your fork, then being unable to push (no write access) — clone **your fork** when contributing.
- Forgetting to sync the fork with `upstream` before starting new work → stale base and conflicts.
- Committing secrets (API keys, `.env`) — public repos are scraped by bots within minutes. Rotate any leaked secret immediately; deleting the commit isn't enough. Set up `.gitignore` before the first commit.
- Opening one giant PR with unrelated changes.
- Pushing straight to `main` on a team repo instead of using a branch + PR.
- Working on the fork's `main` branch for contributions — use a feature branch so `main` can stay a clean mirror of upstream.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[01 - Git Basics|Git Basics]]
- [[02 - Git Branching|Git Branching]]
- [[07 - Git Tags|Git Tags]] — GitHub Releases
- [[09 - Branching Strategies & PR Workflow|Branching Strategies & PR Workflow]]
- [[11 - SSH Keys & GitHub Authentication|SSH Keys & GitHub Authentication]]
