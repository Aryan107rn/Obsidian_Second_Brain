---
tags: [git, github, ssh, authentication, security]
aliases: [SSH, SSH Keys, GitHub SSH, ssh-keygen, Personal Access Token]
created: 2026-10-10
---

# 11 - SSH Keys & GitHub Authentication

## The problem
Pushing to GitHub requires proving who you are. Typing credentials on every `git push` is tedious and passwords are weak; GitHub also **no longer accepts your account password** for Git over HTTPS. You have two proper options: SSH keys or a Personal Access Token (PAT).

## Two ways to connect
| | HTTPS | SSH |
|---|---|---|
| Remote URL | `https://github.com/user/repo.git` | `git@github.com:user/repo.git` |
| Credential | Personal Access Token (or credential manager / `gh auth login`) | Private/public key pair |
| Setup effort | Low | One-time key setup |
| Prompts | Token stored by a credential helper | None once the key is loaded |
| Works through strict firewalls | Usually | Sometimes blocked (port 22) |

SSH is the common choice for daily development; HTTPS+token is fine for quick or CI use.

## How SSH keys work
SSH uses **public-key cryptography**: you generate a **key pair**.
- **Private key** — stays on your machine, never shared. Whoever has it can act as you.
- **Public key** — safe to share; you upload it to GitHub.

When you connect, GitHub challenges your client to prove it holds the private key matching a registered public key. The private key itself never travels over the network.



```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
  "elements": [
    {
      "id": "fea93118c5fa4407be65",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1707354564,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 354539431,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 154.74,
      "y": 147.37,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 427.37,
      "height": 120.84,
      "boundElements": [
        {
          "id": "4474f3ff7db74ab183c5",
          "type": "text"
        }
      ]
    },
    {
      "id": "4474f3ff7db74ab183c5",
      "type": "text",
      "version": 1,
      "versionNonce": 1468763981,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1971787558,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 172.42,
      "y": 162.11,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 392.0,
      "height": 91.37,
      "text": "Your machine (private\nkey)",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "fea93118c5fa4407be65",
      "originalText": "Your machine (private\nkey)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "9fdb7a86dee2446fa99c",
      "type": "line",
      "version": 1,
      "versionNonce": 2020806071,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 667530535,
      "strokeColor": "#94a3b8",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "dashed",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 368.42,
      "y": 271.16,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 1.47,
      "height": 670.53,
      "points": [
        [
          0.0,
          0.0
        ],
        [
          0.0,
          670.53
        ]
      ]
    },
    {
      "id": "b4c728245c134464bc13",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1326500094,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 887034820,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 847.37,
      "y": 147.37,
      "roundness": {
        "type": 3
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 427.37,
      "height": 120.84,
      "boundElements": [
        {
          "id": "44da4818b0ca4019bc1a",
          "type": "text"
        }
      ]
    },
    {
      "id": "44da4818b0ca4019bc1a",
      "type": "text",
      "version": 1,
      "versionNonce": 1583553063,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 591429418,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 865.05,
      "y": 162.11,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 392.0,
      "height": 91.37,
      "text": "GitHub (has your public\nkey)",
      "fontSize": 19,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "b4c728245c134464bc13",
      "originalText": "GitHub (has your public\nkey)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "7b8e306f74024091b90e",
      "type": "line",
      "version": 1,
      "versionNonce": 458943021,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1152372494,
      "strokeColor": "#94a3b8",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "dashed",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1061.05,
      "y": 271.16,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 1.47,
      "height": 670.53,
      "points": [
        [
          0.0,
          0.0
        ],
        [
          0.0,
          670.53
        ]
      ]
    },
    {
      "id": "3640e72c5e2548429d0f",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1372302642,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 394080890,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 368.42,
      "y": 321.26,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 692.63,
      "height": 1.47,
      "points": [
        [
          0.0,
          0.0
        ],
        [
          692.63,
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
      "id": "f071b8d2a17e484d93ca",
      "type": "text",
      "version": 1,
      "versionNonce": 2066219592,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1330174024,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 620.42,
      "y": 268.21,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 188.63,
      "height": 64.84,
      "text": "Connect as\ngit@github.com",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Connect as\ngit@github.com",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "9857bc884dd9469f8df4",
      "type": "arrow",
      "version": 1,
      "versionNonce": 327021071,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1481457640,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 368.42,
      "y": 421.47,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 692.63,
      "height": 1.47,
      "points": [
        [
          692.63,
          0.0
        ],
        [
          0.0,
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
      "id": "9ad72f6abf204ad6aa02",
      "type": "text",
      "version": 1,
      "versionNonce": 1140280135,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 199521307,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 585.05,
      "y": 368.42,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 259.37,
      "height": 64.84,
      "text": "Challenge: prove you\nown the matching key",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Challenge: prove you\nown the matching key",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "5c9bc1701b114acab046",
      "type": "arrow",
      "version": 1,
      "versionNonce": 116680481,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1050980356,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 368.42,
      "y": 521.68,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 692.63,
      "height": 1.47,
      "points": [
        [
          0.0,
          0.0
        ],
        [
          692.63,
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
      "id": "88fa4b5ec79b4617a061",
      "type": "text",
      "version": 1,
      "versionNonce": 173539749,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 626438565,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 596.84,
      "y": 452.42,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 235.79,
      "height": 97.26,
      "text": "Signed response\n(made with private\nkey)",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Signed response\n(made with private\nkey)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "bca7e60d236949678244",
      "type": "arrow",
      "version": 1,
      "versionNonce": 510307758,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2031679071,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1043.37,
      "y": 621.89,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 35.37,
      "height": 1.47,
      "points": [
        [
          35.37,
          0.0
        ],
        [
          0.0,
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
      "id": "fc87dc44c3ef4778b186",
      "type": "text",
      "version": 1,
      "versionNonce": 1185851950,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 713165982,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 943.16,
      "y": 552.63,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 235.79,
      "height": 97.26,
      "text": "Verify signature\nwith stored public\nkey",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Verify signature\nwith stored public\nkey",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
    },
    {
      "id": "397afdfe20914346a866",
      "type": "arrow",
      "version": 1,
      "versionNonce": 611180938,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1346451893,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "dashed",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 368.42,
      "y": 722.11,
      "roundness": {
        "type": 2
      },
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 692.63,
      "height": 1.47,
      "points": [
        [
          692.63,
          0.0
        ],
        [
          0.0,
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
      "id": "1d679c3357d646189b4d",
      "type": "text",
      "version": 1,
      "versionNonce": 1838077436,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1231043620,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 596.84,
      "y": 669.05,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 235.79,
      "height": 64.84,
      "text": "Authenticated, git\npush/pull allowed",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Authenticated, git\npush/pull allowed",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 16.25
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
  "width": 853.9,
  "height": 610.0,
  "previewHeight": 460,
  "previewZoom": 1
}
```



## Setup (Linux / macOS / Git Bash on Windows)
**1. Generate a key** (Ed25519 is the modern recommended type):
```bash
ssh-keygen -t ed25519 -C "you@example.com"
# Press Enter to accept the default path (~/.ssh/id_ed25519)
# Set a passphrase (recommended) — it encrypts the private key on disk
```
Creates `~/.ssh/id_ed25519` (private — keep secret) and `~/.ssh/id_ed25519.pub` (public).

**2. Start the agent and load the key** (so you type the passphrase once per session):
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

**3. Add the public key to GitHub:**
```bash
cat ~/.ssh/id_ed25519.pub     # copy the whole line
```
GitHub → **Settings → SSH and GPG keys → New SSH key** → paste → Save.

**4. Test:**
```bash
ssh -T git@github.com
# Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

**5. Use SSH remotes:**
```bash
git clone git@github.com:user/repo.git
# or switch an existing repo from HTTPS to SSH:
git remote set-url origin git@github.com:user/repo.git
git remote -v
```

## HTTPS with a Personal Access Token (alternative)
1. GitHub → **Settings → Developer settings → Personal access tokens** → generate a token with the minimum scopes needed (prefer fine-grained tokens limited to specific repos).
2. When Git asks for a password, paste the **token** (not your account password).
3. Let a credential helper cache it (`git config --global credential.helper store` or your OS keychain), or use the GitHub CLI: `gh auth login`.

## Security essentials
- **Never share or commit your private key** (`id_ed25519`, no `.pub`). Treat it like a password.
- Use a **passphrase** so a stolen laptop/file isn't instantly usable.
- Treat PATs as passwords too; give them minimal scope and an expiry; never paste them into code or commit them (see [[08 - GitHub Basics|GitHub Basics]] on leaked secrets).
- One key per device so you can revoke a single lost device in GitHub settings.
- If a private key or token leaks: delete it from GitHub immediately and generate a new one.

## Common mistakes
- Uploading the **private** key (or pasting the wrong file) instead of the `.pub` file.
- Cloned via HTTPS but expecting SSH to work — check `git remote -v`; switch with `set-url`.
- `Permission denied (publickey)` — key not added to the agent (`ssh-add -l` to list), public key not uploaded to GitHub, or wrong remote URL.
- Using your account password over HTTPS — rejected; use a token.
- Wrong file permissions on `~/.ssh` (private key must not be readable by others: `chmod 600 ~/.ssh/id_ed25519`).

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]]
- [[01 - Git Basics|Git Basics]] — remotes and `origin`
- [[08 - GitHub Basics|GitHub Basics]] — authenticating, forks, PRs
- [[Linux]] — the shell environment where these commands run
