---
tags: [git, version-control, command-line]
aliases: [Git Undoing Changes, git restore, git reset, git revert, git amend]
created: 2026-08-16
updated: 2026-10-10
---

# 06 - Git Undoing Changes

Git has several undo tools because a change can be at different stages — edited, staged, committed locally, or already pushed. The right tool depends on **how far it has travelled** and **whether others have it**. Some tools rewrite history (safe only for private work); others add new commits (safe everywhere).

## Pick the right tool


```excalidraw
{
  "elements": [
    {
      "id": "9e9ab3e476b445abb16b",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1199622907,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a0",
      "seed": 1671144790,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 280,
      "y": 350,
      "width": 0,
      "height": 80,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          80
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "81ebdffab93d4695b24b",
      "type": "arrow",
      "version": 2,
      "versionNonce": 100775477,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "seed": 1488367245,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 800,
      "y": 350,
      "width": 30,
      "height": 80,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          40
        ],
        [
          -30,
          40
        ],
        [
          -30,
          80
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "24bfa6c0540f4424ab12",
      "type": "arrow",
      "version": 2,
      "versionNonce": 482587547,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a2",
      "seed": 1515021569,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 650,
      "y": 80,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 370,
      "height": 150,
      "points": [
        [
          0,
          0
        ],
        [
          -150,
          0
        ],
        [
          -150,
          90
        ],
        [
          -370,
          90
        ],
        [
          -370,
          150
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "00b48984c0784f6590d7",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1228351381,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a3",
      "seed": 1167538353,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 800,
      "y": 120,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 0,
      "height": 110,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          110
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "8a6accc540f84b3e89d5",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1725990971,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a4",
      "seed": 1083036097,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 950,
      "y": 80,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 350,
      "height": 150,
      "points": [
        [
          0,
          0
        ],
        [
          150,
          0
        ],
        [
          150,
          90
        ],
        [
          350,
          90
        ],
        [
          350,
          150
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "2UvZPl8M",
      "type": "text",
      "version": 2,
      "versionNonce": 1270466779,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a6",
      "seed": 1439185832,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 84,
      "y": 643.25,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 106,
      "height": 17.5,
      "text": "No — edited",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "No — edited",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "6284b61c36de444eb81c",
      "type": "arrow",
      "version": 60,
      "versionNonce": 143467131,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a7",
      "seed": 1054082088,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 280,
      "y": 560,
      "roundness": {
        "type": 2
      },
      "updated": 1793216425629,
      "link": null,
      "locked": false,
      "width": 96.22466424593819,
      "height": 149.4735468705161,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          75
        ],
        [
          69.45092174819871,
          101.18592345122295
        ],
        [
          96.22466424593819,
          149.4735468705161
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": {
        "elementId": "d0991f8996bc4ef2abbd",
        "mode": "orbit",
        "fixedPoint": [
          0.7217120658340087,
          0.27828793416599135
        ],
        "focus": 0
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "MqhT5EaG",
      "type": "text",
      "version": 2,
      "versionNonce": 283243899,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a8",
      "seed": 1643980815,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 300,
      "y": 603.25,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 114,
      "height": 17.5,
      "text": "Yes — staged",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Yes — staged",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "6d614a8f26604827876e",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1585345461,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a9",
      "seed": 693453869,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 620,
      "y": 495,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 50,
      "height": 215,
      "points": [
        [
          0,
          0
        ],
        [
          -50,
          0
        ],
        [
          -50,
          170
        ],
        [
          -35,
          170
        ],
        [
          -35,
          215
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "XwgoEVgW",
      "type": "text",
      "version": 2,
      "versionNonce": 630951451,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aA",
      "seed": 171822983,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 430,
      "y": 638.25,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 146,
      "height": 17.5,
      "text": "Yes, keep staged",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Yes, keep staged",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "78582fb5e895430e8b2d",
      "type": "arrow",
      "version": 2,
      "versionNonce": 1044395285,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aB",
      "seed": 54037334,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 770,
      "y": 560,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 0,
      "height": 150,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          150
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "DWqaQRjm",
      "type": "text",
      "version": 2,
      "versionNonce": 1826165435,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aC",
      "seed": 1178152272,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 790,
      "y": 628.25,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 162,
      "height": 17.5,
      "text": "Yes, keep unstaged",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "Yes, keep unstaged",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "31ead2c0b08c48ef9087",
      "type": "arrow",
      "version": 2,
      "versionNonce": 922929781,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aD",
      "seed": 597297360,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 920,
      "y": 495,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 205,
      "height": 215,
      "points": [
        [
          0,
          0
        ],
        [
          30,
          0
        ],
        [
          30,
          170
        ],
        [
          205,
          170
        ],
        [
          205,
          215
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "uL67I7R2",
      "type": "text",
      "version": 2,
      "versionNonce": 1195180891,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aE",
      "seed": 1427168403,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 980,
      "y": 638.25,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 106,
      "height": 17.5,
      "text": "No, discard",
      "fontSize": 18,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": null,
      "originalText": "No, discard",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 17,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "2bef6a2e457d43bbb945",
      "type": "arrow",
      "version": 2,
      "versionNonce": 428585941,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aF",
      "seed": 509787053,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1300,
      "y": 350,
      "roundness": {
        "type": 2
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 0,
      "height": 160,
      "points": [
        [
          0,
          0
        ],
        [
          0,
          160
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": null,
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "966e087fb4cb4e0f9df9",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1561976827,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aG",
      "seed": 1580053403,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 650,
      "y": 35,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 85,
      "boundElements": [
        {
          "id": "VrbSRieZ",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "VrbSRieZ",
      "type": "text",
      "version": 4,
      "versionNonce": 1173699445,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aH",
      "seed": 1907924665,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 692.4497299194336,
      "y": 66.875,
      "roundness": null,
      "updated": 1793216395141,
      "link": null,
      "locked": false,
      "width": 215.1005401611328,
      "height": 21.25,
      "text": "I want to undo something",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "966e087fb4cb4e0f9df9",
      "originalText": "I want to undo something",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "fde76d5c87c04b4eba13",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 678570139,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aI",
      "seed": 117995882,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 60,
      "y": 230,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 440,
      "height": 120,
      "boundElements": [
        {
          "id": "JEuvStqc",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "JEuvStqc",
      "type": "text",
      "version": 4,
      "versionNonce": 1505403445,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aJ",
      "seed": 1823746982,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 193.3936996459961,
      "y": 279.375,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 173.2126007080078,
      "height": 21.25,
      "text": "Uncommitted changes",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "fde76d5c87c04b4eba13",
      "originalText": "Uncommitted changes",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "489f47b0b080439b99bd",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1056464187,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aK",
      "seed": 1611460867,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 580,
      "y": 230,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 440,
      "height": 120,
      "boundElements": [
        {
          "id": "FDx32SZd",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "FDx32SZd",
      "type": "text",
      "version": 4,
      "versionNonce": 786782453,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aL",
      "seed": 291908683,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 669.2023010253906,
      "y": 258.125,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 261.59539794921875,
      "height": 63.75,
      "text": "Local commit\nFor last message / missed file:\ngit commit --amend",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "489f47b0b080439b99bd",
      "originalText": "Local commit\nFor last message / missed file:\ngit commit --amend",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "b69dc5ff69e74e66bc0a",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1544486363,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aM",
      "seed": 446877763,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1080,
      "y": 230,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 440,
      "height": 120,
      "boundElements": [
        {
          "id": "yAWk73A7",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "yAWk73A7",
      "type": "text",
      "version": 4,
      "versionNonce": 140599221,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aN",
      "seed": 1179027224,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1241.171630859375,
      "y": 279.375,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 117.65673828125,
      "height": 21.25,
      "text": "Shared commit",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "b69dc5ff69e74e66bc0a",
      "originalText": "Shared commit",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "6d10c1f45c2b477d8426",
      "type": "diamond",
      "version": 2,
      "versionNonce": 717785723,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aO",
      "seed": 310035503,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 130,
      "y": 430,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 130,
      "boundElements": [
        {
          "id": "GA9jUkYj",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "GA9jUkYj",
      "type": "text",
      "version": 4,
      "versionNonce": 1578808949,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aP",
      "seed": 719751694,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 222.79512786865234,
      "y": 484.375,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 114.40974426269531,
      "height": 21.25,
      "text": "Is it staged?",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "6d10c1f45c2b477d8426",
      "originalText": "Is it staged?",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "0435a1a7b89e4bbbb122",
      "type": "rectangle",
      "version": 38,
      "versionNonce": 1736057429,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aQ",
      "seed": 1014297528,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": -88.37241010268619,
      "y": 717.4878800078194,
      "roundness": {
        "type": 3
      },
      "updated": 1793216408588,
      "link": null,
      "locked": false,
      "width": 270,
      "height": 90,
      "boundElements": [
        {
          "id": "HF68YGAX",
          "type": "text"
        },
        {
          "id": "b059f47de3fd498c95f7",
          "type": "arrow"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "b059f47de3fd498c95f7",
      "type": "arrow",
      "version": 196,
      "versionNonce": 696008277,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aQV",
      "seed": 2082665600,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 130,
      "y": 495,
      "roundness": {
        "type": 2
      },
      "updated": 1793216416902,
      "link": null,
      "locked": false,
      "width": 82.1388681664044,
      "height": 223.31354545946863,
      "points": [
        [
          0,
          0
        ],
        [
          -50,
          0
        ],
        [
          -82.1388681664044,
          78.62507042202606
        ],
        [
          -74.20994722501764,
          148.97907175392686
        ],
        [
          -31.280333577336762,
          223.31354545946863
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": null,
      "endBinding": {
        "elementId": "0435a1a7b89e4bbbb122",
        "mode": "inside",
        "fixedPoint": [
          0.6929336167605534,
          0.009174060573880323
        ],
        "focus": 0
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "HF68YGAX",
      "type": "text",
      "version": 39,
      "versionNonce": 1466622939,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aR",
      "seed": 40600186,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": -16.408275886377595,
      "y": 751.8628800078194,
      "roundness": null,
      "updated": 1793216405997,
      "link": null,
      "locked": false,
      "width": 126.07173156738281,
      "height": 21.25,
      "text": "git restore file",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "0435a1a7b89e4bbbb122",
      "originalText": "git restore file",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "d0991f8996bc4ef2abbd",
      "type": "rectangle",
      "version": 40,
      "versionNonce": 962140955,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aS",
      "seed": 50370601,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 198.5762441001567,
      "y": 715.4735468705161,
      "roundness": {
        "type": 3
      },
      "updated": 1793216425629,
      "link": null,
      "locked": false,
      "width": 270,
      "height": 90,
      "boundElements": [
        {
          "id": "obVyqS43",
          "type": "text"
        },
        {
          "id": "6284b61c36de444eb81c",
          "type": "arrow"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "obVyqS43",
      "type": "text",
      "version": 41,
      "versionNonce": 179822293,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aT",
      "seed": 1307943720,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 230.47146962505906,
      "y": 749.8485468705161,
      "roundness": null,
      "updated": 1793216419331,
      "link": null,
      "locked": false,
      "width": 206.2095489501953,
      "height": 21.25,
      "text": "git restore --staged file",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d0991f8996bc4ef2abbd",
      "originalText": "git restore --staged file",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "1d6a0b67bccd4033849f",
      "type": "diamond",
      "version": 2,
      "versionNonce": 214939739,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aU",
      "seed": 491332069,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 620,
      "y": 430,
      "roundness": null,
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 300,
      "height": 130,
      "boundElements": [
        {
          "id": "yoEnvYyH",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "yoEnvYyH",
      "type": "text",
      "version": 4,
      "versionNonce": 1391368885,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aV",
      "seed": 1736176422,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 733.3990783691406,
      "y": 473.75,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 73.20184326171875,
      "height": 42.5,
      "text": "Keep the\nchanges?",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "1d6a0b67bccd4033849f",
      "originalText": "Keep the changes?",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "1daf7cccff154a2c8532",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 680391931,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aW",
      "seed": 1792122398,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 470,
      "y": 710,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 230,
      "height": 90,
      "boundElements": [
        {
          "id": "l5ROeMeR",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "l5ROeMeR",
      "type": "text",
      "version": 4,
      "versionNonce": 752169333,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aX",
      "seed": 824808939,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 481.6402282714844,
      "y": 744.375,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 206.71954345703125,
      "height": 21.25,
      "text": "git reset --soft HEAD~1",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "1daf7cccff154a2c8532",
      "originalText": "git reset --soft HEAD~1",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "8b703772fd1f4240b2ee",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1989909915,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aY",
      "seed": 905816193,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 740,
      "y": 710,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 230,
      "height": 90,
      "boundElements": [
        {
          "id": "i87gzXcp",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "i87gzXcp",
      "type": "text",
      "version": 4,
      "versionNonce": 898963509,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aZ",
      "seed": 2063143916,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 781.1351623535156,
      "y": 733.75,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 147.72967529296875,
      "height": 42.5,
      "text": "git reset HEAD~1\n(mixed)",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "8b703772fd1f4240b2ee",
      "originalText": "git reset HEAD~1\n(mixed)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "b0a95cfd5b214fa6bd3a",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1378562619,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aa",
      "seed": 1417378685,
      "strokeColor": "#334155",
      "backgroundColor": "#fee2e2",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1010,
      "y": 710,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 230,
      "height": 90,
      "boundElements": [
        {
          "id": "d5q6kDol",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "d5q6kDol",
      "type": "text",
      "version": 4,
      "versionNonce": 1662668533,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "ab",
      "seed": 1239378417,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1021.5382308959961,
      "y": 744.375,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 206.9235382080078,
      "height": 21.25,
      "text": "git reset --hard HEAD~1",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "b0a95cfd5b214fa6bd3a",
      "originalText": "git reset --hard HEAD~1",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    },
    {
      "id": "9f19e80ee0d346739637",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 203371227,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "ac",
      "seed": 1482455268,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1170,
      "y": 510,
      "roundness": {
        "type": 3
      },
      "updated": 1793216395060,
      "link": null,
      "locked": false,
      "width": 260,
      "height": 90,
      "boundElements": [
        {
          "id": "G92XBcqR",
          "type": "text"
        }
      ],
      "hasTextLink": false
    },
    {
      "id": "G92XBcqR",
      "type": "text",
      "version": 4,
      "versionNonce": 542168501,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "ad",
      "seed": 1484496909,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2.4,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 1228.3196563720703,
      "y": 544.375,
      "roundness": null,
      "updated": 1793216395142,
      "link": null,
      "locked": false,
      "width": 143.36068725585938,
      "height": 21.25,
      "text": "git revert commit",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "9f19e80ee0d346739637",
      "originalText": "git revert commit",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 21,
      "boundElements": [],
      "hasTextLink": false
    }
  ],
  "appState": {
    "viewBackgroundColor": "#00000000"
  },
  "files": {},
  "previewHeight": 460,
  "previewZoom": 1
}
```



## Quick reference
```bash
# Working directory / staging area
git restore file                  # discard unstaged edits to a file (cannot be undone!)
git restore .                     # discard ALL unstaged edits
git restore --staged file         # unstage, keep the edits
git clean -n                      # DRY RUN: list untracked files that would be deleted
git clean -fd                     # delete untracked files and folders (irreversible)

# Last commit
git commit --amend                         # edit the last commit's message
git commit --amend --no-edit               # add newly staged changes to the last commit

# Moving back through commits (rewrites history)
git reset --soft  HEAD~1          # undo commit, keep changes STAGED
git reset         HEAD~1          # (--mixed, default) undo commit, keep changes UNSTAGED
git reset --hard  HEAD~1          # undo commit, DISCARD changes

# Shared history (does not rewrite anything)
git revert <hash>                 # new commit that reverses <hash>
git revert -m 1 <merge-hash>      # reverse a merge commit (-m picks which parent is "mainline")
```

## Situation table
| Situation | Command |
|---|---|
| Edited a file, want the old version back | `git restore file` |
| Staged a file by mistake | `git restore --staged file` |
| Typo in last commit message (not pushed) | `git commit --amend` |
| Forgot to include a file in last commit (not pushed) | `git add file` → `git commit --amend --no-edit` |
| Local commit wrong, keep the work | `git reset --soft HEAD~1` |
| Local commit wrong, throw it away | `git reset --hard HEAD~1` |
| Bad commit already pushed / on `main` | `git revert <hash>` |

## `reset`: soft vs mixed vs hard
`reset` moves the current branch pointer back; the flag decides what happens to your files.
| Mode | Branch pointer | Staging area | Working directory |
|---|---|---|---|
| `--soft` | moved back | **kept** (changes staged) | kept |
| `--mixed` (default) | moved back | reset (changes unstaged) | kept |
| `--hard` | moved back | reset | **reset — changes lost** |

## The Rule
`reset --hard`, `commit --amend`, and rebase all **rewrite history**. Do that only to commits **nobody else has pulled**. Once a commit is pushed and shared, use `git revert`: it adds a *new* commit that cancels the old one, leaving everyone's history intact. This is the same principle as the Golden Rule in [[04 - Git Rebase|Git Rebase]].

## Safety net: reflog
Even after `reset --hard`, the old commits usually still exist for a while (Git doesn't delete them immediately). `git reflog` lists where HEAD has been, so you can find the lost hash and recover it — see [[10 - Git Reflog, Cherry-pick & Recovery|Reflog & Recovery]]. The exception: uncommitted edits destroyed by `git restore`/`reset --hard` or `git clean` were never in Git, so they are **not** recoverable.

## Common mistakes
- `reset --hard` out of habit without checking whether the commit was pushed — then force-pushing and breaking teammates' history.
- Confusing `restore` (files/staging) with `reset` (branch pointer/commits) — they act at different stages of the [[01 - Git Basics|four-area flow]].
- Running `git clean -fd` without `-n` first — permanently deletes untracked files.
- Amending a commit that's already pushed — creates a diverged history and forces a force-push.
- Forgetting reflog exists and giving up on "lost" commits.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[01 - Git Basics|Git Basics]]
- [[04 - Git Rebase|Git Rebase]] — shares the "don't rewrite shared history" rule
- [[05 - Git Stash|Git Stash]] — parks work instead of undoing it
- [[10 - Git Reflog, Cherry-pick & Recovery|Reflog & Recovery]]
