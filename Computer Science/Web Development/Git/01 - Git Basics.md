---
tags: [git, version-control, command-line]
aliases: [Git Basics, git setup, git commands]
created: 2026-08-16
updated: 2026-10-10
---

# 01 - Git Basics

## What is Git?
Git is a **distributed version control system (VCS)** — it records changes to files over time so you can save snapshots (commits), return to any earlier state, and collaborate without overwriting each other's work.

**Distributed** means every clone holds the *entire* project history, not just the latest files. Unlike older centralized systems (SVN, CVS), you can commit, branch, and browse history fully offline; the server is just another copy.

## Why version control at all?
Without it, history lives in files like `final_v2_REAL_final.zip`. Git gives you: an exact record of *what* changed, *when*, *who* changed it and (via the commit message) *why*; safe experimentation on branches; and the ability to undo mistakes.

## First-time setup
```bash
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main     # new repos start on `main`, not `master`
git config --list                                # verify

git init                    # turn the current folder into a repo (creates .git/)
git clone <url>             # copy an existing remote repo (history included)
```
The name/email are stamped on every commit you make; GitHub uses the email to attribute commits to your account.

## The four areas (the mental model everything else builds on)


```excalidraw
{
  "elements": [
    {
      "id": "3e70e3582165474c9c70",
      "type": "text",
      "version": 2,
      "versionNonce": 955065757,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a0",
      "seed": 1751977779,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 490,
      "y": 507,
      "roundness": null,
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 150,
      "height": 24,
      "text": "git pull = fetch + merge",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git pull = fetch + merge",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "419dd3074ac145b2b319",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1947684531,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "seed": 399113897,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1085,
      "y": 298,
      "roundness": {
        "type": 2
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 900,
      "height": 237,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          237
        ],
        [
          -900,
          237
        ],
        [
          -900,
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
      "id": "e840e988590e49f382cc",
      "type": "text",
      "version": 2,
      "versionNonce": 1993370109,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a2",
      "seed": 1756626443,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 860,
      "y": 397,
      "roundness": null,
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 150,
      "height": 24,
      "text": "git fetch",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git fetch",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "23cca50197bc4fc28076",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1816199251,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a3",
      "seed": 1827700953,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1085,
      "y": 298,
      "roundness": {
        "type": 2
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 127,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          127
        ],
        [
          -300,
          127
        ],
        [
          -300,
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
      "id": "771535d844c547a79b0d",
      "type": "text",
      "version": 38,
      "versionNonce": 1429823677,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a4",
      "seed": 458564507,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 859.3740234375,
      "y": 199.63739013671875,
      "roundness": null,
      "updated": 1793456215733,
      "link": null,
      "locked": false,
      "width": 150,
      "height": 20,
      "text": "git push",
      "fontSize": 16,
      "fontFamily": 8,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git push",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "3a22bb9d7b7f45118be0",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1132690931,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a5",
      "seed": 1661755066,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 895,
      "y": 239,
      "roundness": {
        "type": 2
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 80,
      "height": 0,
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
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "48cdb534647a4ddc9939",
      "type": "text",
      "version": 61,
      "versionNonce": 1741724445,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a6",
      "seed": 1877824202,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 559.39794921875,
      "y": 209.83856201171875,
      "roundness": null,
      "updated": 1793456213675,
      "link": null,
      "locked": false,
      "width": 150,
      "height": 20,
      "text": "git commit",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git commit",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "a41a282b542e418193fa",
      "type": "arrow",
      "version": 2,
      "versionNonce": 887441299,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a7",
      "seed": 2111794376,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 595,
      "y": 239,
      "roundness": {
        "type": 2
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 80,
      "height": 0,
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
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "a24217f614aa4ed097bf",
      "type": "text",
      "version": 116,
      "versionNonce": 275576093,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a8",
      "seed": 1715119801,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 258.8811340332031,
      "y": 192.52838134765625,
      "roundness": null,
      "updated": 1793456211259,
      "link": null,
      "locked": false,
      "width": 150,
      "height": 20,
      "text": "git add",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "git add",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": []
    },
    {
      "id": "02a157fd1c3342298069",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1453120819,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a9",
      "seed": 1639100291,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 295,
      "y": 239,
      "roundness": {
        "type": 2
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 80,
      "height": 0,
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
      "endArrowhead": "arrow",
      "boundElements": []
    },
    {
      "id": "da5dc9bab46e46348112",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 336659325,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aA",
      "seed": 1208231981,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 75,
      "y": 180,
      "roundness": {
        "type": 3
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 118,
      "boundElements": [
        {
          "id": "6df355bd3d3945fbb5bf",
          "type": "text"
        }
      ]
    },
    {
      "id": "6df355bd3d3945fbb5bf",
      "type": "text",
      "version": 2,
      "versionNonce": 873512659,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aB",
      "seed": 1433994405,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 89,
      "y": 198,
      "roundness": null,
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 82,
      "text": "Working directory\n(files on disk)",
      "fontSize": 22,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "da5dc9bab46e46348112",
      "originalText": "Working directory\n(files on disk)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 23,
      "boundElements": []
    },
    {
      "id": "439def1b8e184ffea15a",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1635617757,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aC",
      "seed": 1779528694,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 375,
      "y": 180,
      "roundness": {
        "type": 3
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 118,
      "boundElements": [
        {
          "id": "196e8abfc4da43f796f9",
          "type": "text"
        }
      ]
    },
    {
      "id": "196e8abfc4da43f796f9",
      "type": "text",
      "version": 2,
      "versionNonce": 668429427,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aD",
      "seed": 386686917,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 389,
      "y": 198,
      "roundness": null,
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 82,
      "text": "Staging area / Index\n(next commit contents)",
      "fontSize": 22,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "439def1b8e184ffea15a",
      "originalText": "Staging area / Index\n(next commit contents)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 23,
      "boundElements": []
    },
    {
      "id": "3104e821245c4220b22a",
      "type": "rectangle",
      "version": 37,
      "versionNonce": 1206865629,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aE",
      "seed": 1988090828,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 678.0112915039062,
      "y": 183.81942749023438,
      "roundness": {
        "type": 3
      },
      "updated": 1793456175412,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 118,
      "boundElements": [
        {
          "id": "1b194501c1144f05810c",
          "type": "text"
        }
      ]
    },
    {
      "id": "1b194501c1144f05810c",
      "type": "text",
      "version": 37,
      "versionNonce": 1312516925,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aF",
      "seed": 2103368120,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 692.0112915039062,
      "y": 201.81942749023438,
      "roundness": null,
      "updated": 1793456175412,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 82,
      "text": "Local repository\n(.git history)",
      "fontSize": 22,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "3104e821245c4220b22a",
      "originalText": "Local repository\n(.git history)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 23,
      "boundElements": []
    },
    {
      "id": "4439638d71504213866e",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1885862045,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aG",
      "seed": 437828121,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 975,
      "y": 180,
      "roundness": {
        "type": 3
      },
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 220,
      "height": 118,
      "boundElements": [
        {
          "id": "2ff07147f8ff4589affd",
          "type": "text"
        }
      ]
    },
    {
      "id": "2ff07147f8ff4589affd",
      "type": "text",
      "version": 2,
      "versionNonce": 614935475,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aH",
      "seed": 1638312638,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.9,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 989,
      "y": 198,
      "roundness": null,
      "updated": 1793456163201,
      "link": null,
      "locked": false,
      "width": 192,
      "height": 82,
      "text": "Remote\n(GitHub)",
      "fontSize": 22,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "4439638d71504213866e",
      "originalText": "Remote\n(GitHub)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 23,
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


- **Working directory** — your files exactly as they sit on disk now.
- **Staging area (index)** — a draft of the *next* commit. `git add` copies the current state of a file into it.
- **Local repository (`.git`)** — the permanent, committed history on your machine.
- **Remote** — a hosted copy (e.g. GitHub), synced with `push`/`fetch`/`pull`.

**Why a staging area exists:** it lets you build a commit out of only *some* of your changes. Fixed two unrelated bugs in one sitting? Stage and commit them separately so each commit is one clean, reviewable, revertable idea.

## Daily flow
```bash
git status                  # what changed? what's staged? which branch am I on?
git add file.js             # stage one file
git add .                   # stage everything in the current directory (check status first!)
git add -p                  # stage interactively, hunk by hunk
git commit -m "add login form validation"
git commit -am "msg"        # stage + commit already-tracked files in one step (does NOT include new files)
git push                    # upload commits to the remote
git pull                    # download + merge remote commits
```

### Writing good commit messages
- One logical change per commit; imperative, present tense: `fix null check in parser`, not `fixed stuff`.
- Short summary line (~50 chars), optional blank line, then a body explaining *why* if it isn't obvious.
- Many teams use **Conventional Commits** prefixes: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:` — e.g. `feat: add JWT login route`. These make history scannable and can drive automatic changelogs/versioning (see [[07 - Git Tags]]).

## Viewing history and changes
```bash
git log                          # full history
git log --oneline --graph --all  # compact, visual, all branches
git log -p file.js               # history of one file, with diffs
git diff                         # unstaged changes (working dir vs index)
git diff --staged                # staged changes (index vs last commit)
git show <hash>                  # one commit's message + diff
git blame file.js                # who last changed each line, and in which commit
```

## Remotes
```bash
git remote -v                          # list remotes
git remote add origin <url>            # connect a local repo to a remote
git push -u origin main                # first push; -u sets the upstream so later `git push`/`pull` need no arguments
git fetch                              # download new commits, change nothing locally
git pull                               # fetch + merge
git pull --rebase                      # fetch + rebase (linear history)
```
See [[04 - Git Rebase|Git Rebase]] for what `--rebase` does and when it is safe.
**Fetch vs pull:** `fetch` is safe — it only updates your view of the remote so you can inspect (`git log origin/main`) before combining. `pull` = `fetch` + `merge` immediately, which can surprise you with conflicts.

## Ignoring files: `.gitignore`
A `.gitignore` file in the repo root lists patterns Git should never track:
```gitignore
node_modules/        # dependencies — reinstallable from package.json
.env                 # secrets / local config
dist/                # build output
*.log
.DS_Store
!important.log       # a leading ! re-includes something a broader pattern excluded
```
- Create it **before the first commit** — it only affects *untracked* files.
- If a file is already tracked, adding it to `.gitignore` does nothing; untrack it first: `git rm --cached <file>` (keeps it on disk) then commit.
- Debug a rule with `git check-ignore -v <path>`.
- Typical Node/Express projects ignore `node_modules/` and `.env` (see [[03 - npm, package.json & Node Modules]]); commit `package-lock.json`, not `node_modules`.

## Under the hood (brief)
The `.git/` folder *is* the repository. Key parts: `objects/` (all content, stored by hash), `refs/` (branch and tag pointers), `HEAD` (which branch/commit you're on), `config`.
- Git stores **snapshots**, not diffs: each commit points to a full tree of the project, reusing unchanged files by reference so it stays small.
- Every object is named by a **hash** (SHA-1 today) of its content — change anything and the hash changes, which is why history is tamper-evident and why rewriting history (rebase/reset) produces *new* commits.

## Key terms
- **Commit** — an immutable snapshot with an author, timestamp, message, and pointer(s) to its parent commit(s).
- **Branch** — a movable pointer to a commit (see [[02 - Git Branching|Git Branching]]).
- **HEAD** — pointer to where you are now, normally the tip of the checked-out branch.
- **origin** — the conventional default name for your main remote.

## Common mistakes
- `git add .` without checking `git status` first — accidentally staging build output, secrets, or unrelated edits.
- Vague commit messages ("update", "fix stuff") — makes `git log`/`git blame` useless later.
- Not pulling before starting work — avoidable merge conflicts when your branch has diverged.
- Committing secrets (`.env`, API keys). Removing them in a later commit does **not** remove them from history; treat them as leaked and rotate them.
- Running `git init` inside an existing repo or in the wrong folder (e.g. your home directory) — check `git status`/`pwd` first.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[02 - Git Branching|Git Branching]]
- [[06 - Git Undoing Changes|Git Undoing Changes]]
- [[08 - GitHub Basics|GitHub Basics]]
- [[Linux]] — the command-line environment Git is usually run from
