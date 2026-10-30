---
tags: [git, version-control, command-line]
aliases: [Git Rebase, rebase, git rebase, interactive rebase]
created: 2026-08-08
updated: 2026-10-10
---

# 04 - Git Rebase

## What is rebase?
**Rebase** takes the commits unique to your branch and **replays them on top of another branch's tip**, producing a **linear history** instead of a merge commit. The content of each commit is the same, but each is re-created with a **new parent and a new hash**.

## Merge vs Rebase
Starting point: `feature` diverged from `main` at commit B. Feature has C, D; `main` moved on to E.

### 1. Merge — preserves history, adds a merge commit


```excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
  "elements": [
    {
      "id": "120d77c18a914bdaa262",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1816356089,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1764777023,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "id": "bbf5af6715924444a600",
      "type": "arrow",
      "version": 1,
      "versionNonce": 1331386260,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2135194318,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "id": "2c0f1c764db34b98b555",
      "type": "arrow",
      "version": 1,
      "versionNonce": 301292058,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1539810447,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "id": "446c9fc66f2249c192c3",
      "type": "text",
      "version": 1,
      "versionNonce": 1627009032,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 93696597,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "fontSize": 13,
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
      "id": "d21a46c8f14a4b9cacde",
      "type": "arrow",
      "version": 1,
      "versionNonce": 475163030,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1822738622,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "id": "4fbcf82bcf8e426a82f1",
      "type": "text",
      "version": 1,
      "versionNonce": 20945187,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2131478421,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "fontSize": 13,
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
      "id": "e05933282c0c47f4a0a4",
      "type": "arrow",
      "version": 1,
      "versionNonce": 831125374,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1432376056,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 680,
      "y": 244,
      "roundness": {
        "type": 2
      },
      "updated": 1,
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
      "endArrowhead": "arrow"
    },
    {
      "id": "b401a9b4573c47e09f1f",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1648236711,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 2122416276,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
          "id": "872096d5c262458bb150",
          "type": "text"
        }
      ]
    },
    {
      "id": "872096d5c262458bb150",
      "type": "text",
      "version": 1,
      "versionNonce": 170043604,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 322353104,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "text": "A",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "b401a9b4573c47e09f1f",
      "originalText": "A",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "b59b9e4554a04828abba",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1522227640,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 896591378,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
          "id": "e93482d0ae994a42bfba",
          "type": "text"
        }
      ]
    },
    {
      "id": "e93482d0ae994a42bfba",
      "type": "text",
      "version": 1,
      "versionNonce": 1919976849,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 58979407,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "text": "B",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "b59b9e4554a04828abba",
      "originalText": "B",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "7b1d3b80465a4aa4b359",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1983866913,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 112771598,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
          "id": "d3e613b00764415aa5a7",
          "type": "text"
        }
      ]
    },
    {
      "id": "d3e613b00764415aa5a7",
      "type": "text",
      "version": 1,
      "versionNonce": 1367125726,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 60311672,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "text": "C",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "7b1d3b80465a4aa4b359",
      "originalText": "C",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "0b010f6cfbc240578a31",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 692399007,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 325079141,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
          "id": "b1da7b5d70a247ab8cb7",
          "type": "text"
        }
      ]
    },
    {
      "id": "b1da7b5d70a247ab8cb7",
      "type": "text",
      "version": 1,
      "versionNonce": 598269138,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 937956938,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "text": "D",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "0b010f6cfbc240578a31",
      "originalText": "D",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "ea73f8475dd646d08cfd",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1488010799,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1084722564,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
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
          "id": "1832debd43114439acbf",
          "type": "text"
        }
      ]
    },
    {
      "id": "1832debd43114439acbf",
      "type": "text",
      "version": 1,
      "versionNonce": 1769607527,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 943740169,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622,
      "y": 220,
      "roundness": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "E",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "ea73f8475dd646d08cfd",
      "originalText": "E",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "dd1b821090e745838424",
      "type": "rectangle",
      "version": 1,
      "versionNonce": 1168512174,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 105854619,
      "strokeColor": "#334155",
      "backgroundColor": "#ffedd5",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
          "id": "52bfc2d966d24e6ead73",
          "type": "text"
        }
      ]
    },
    {
      "id": "52bfc2d966d24e6ead73",
      "type": "text",
      "version": 1,
      "versionNonce": 963674048,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 600523020,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "text": "M (merge)",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "dd1b821090e745838424",
      "originalText": "M (merge)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20.0
    },
    {
      "id": "1a0a2dcabf1f40e88947",
      "type": "text",
      "version": 1,
      "versionNonce": 881602735,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 1519630963,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "right",
      "verticalAlign": "middle",
      "originalText": "main",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20
    },
    {
      "id": "84f32349c2044cd984e1",
      "type": "text",
      "version": 1,
      "versionNonce": 229935349,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": null,
      "seed": 798896852,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
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
      "text": "feature",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "right",
      "verticalAlign": "middle",
      "originalText": "feature",
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



### 2. Rebase — replays commits linearly


```excalidraw
{
  "elements": [
    {
      "id": "65119dd50f4e4b59b5fc",
      "type": "arrow",
      "version": 2,
      "versionNonce": 781122222,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a0",
      "seed": 846177885,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 260,
      "y": 244,
      "roundness": {
        "type": 2
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 140,
      "height": 0,
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
      "id": "47cd0402de214f91888b",
      "type": "arrow",
      "version": 2,
      "versionNonce": 32886578,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "seed": 1257103231,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 680,
      "y": 464,
      "roundness": {
        "type": 2
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 140,
      "height": 0,
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
      "id": "5fd56dbf2b724bb1bce0",
      "type": "arrow",
      "version": 2,
      "versionNonce": 477590766,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a2",
      "seed": 1322703826,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 434,
      "y": 278,
      "roundness": {
        "type": 2
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 0,
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
      "id": "2d86529690db4783bd85",
      "type": "text",
      "version": 2,
      "versionNonce": 546904306,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a3",
      "seed": 721122226,
      "strokeColor": "#475569",
      "backgroundColor": "#ffffff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 402,
      "y": 330,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 64,
      "height": 22,
      "text": "branch",
      "fontSize": 13,
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
      "id": "9b376a7c80524664b10d",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 601270062,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a4",
      "seed": 668324036,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 190,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "7e15c3523ddf4704b002",
          "type": "text"
        }
      ]
    },
    {
      "id": "7e15c3523ddf4704b002",
      "type": "text",
      "version": 2,
      "versionNonce": 665660082,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a5",
      "seed": 456579593,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 202,
      "y": 220,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "A",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "9b376a7c80524664b10d",
      "originalText": "A",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "be959df6f8a64f0aaf68",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1427476846,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a6",
      "seed": 1534904671,
      "strokeColor": "#334155",
      "backgroundColor": "#dbeafe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 400,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "d4a01f636dbf4ccaaa20",
          "type": "text"
        }
      ]
    },
    {
      "id": "d4a01f636dbf4ccaaa20",
      "type": "text",
      "version": 2,
      "versionNonce": 1574503538,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a7",
      "seed": 1599576299,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 412,
      "y": 220,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "B",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "be959df6f8a64f0aaf68",
      "originalText": "B",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "221dfbb10f4d485ab9f8",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1266496430,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a8",
      "seed": 341097823,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 430,
      "roundness": {
        "type": 3
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "2f0982244fb3463d8335",
          "type": "text"
        }
      ]
    },
    {
      "id": "2f0982244fb3463d8335",
      "type": "text",
      "version": 2,
      "versionNonce": 484681266,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "a9",
      "seed": 916540063,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622,
      "y": 440,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "C' (rebased)",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "221dfbb10f4d485ab9f8",
      "originalText": "C' (rebased)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "d421c1df06e84c4d85e0",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 1362529774,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aA",
      "seed": 86641468,
      "strokeColor": "#334155",
      "backgroundColor": "#ede9fe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 820,
      "y": 430,
      "roundness": {
        "type": 3
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "f910a7ac8a024105bf4d",
          "type": "text"
        }
      ]
    },
    {
      "id": "f910a7ac8a024105bf4d",
      "type": "text",
      "version": 2,
      "versionNonce": 1840517106,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aB",
      "seed": 631914464,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 832,
      "y": 440,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "D' (rebased)",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d421c1df06e84c4d85e0",
      "originalText": "D' (rebased)",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "ac1c5b4cbe9e43f28d57",
      "type": "rectangle",
      "version": 2,
      "versionNonce": 55222318,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aC",
      "seed": 1288063990,
      "strokeColor": "#334155",
      "backgroundColor": "#dcfce7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 610,
      "y": 210,
      "roundness": {
        "type": 3
      },
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 136,
      "height": 76,
      "boundElements": [
        {
          "id": "03e0d6a93496446992bd",
          "type": "text"
        }
      ]
    },
    {
      "id": "03e0d6a93496446992bd",
      "type": "text",
      "version": 2,
      "versionNonce": 609305010,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aD",
      "seed": 2139046393,
      "strokeColor": "#0f172a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 622,
      "y": 220,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 112,
      "height": 56,
      "text": "E",
      "fontSize": 16,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "ac1c5b4cbe9e43f28d57",
      "originalText": "E",
      "autoResize": false,
      "lineHeight": 1.25,
      "baseline": 20,
      "boundElements": []
    },
    {
      "id": "b433ec74d9ea436a85a3",
      "type": "text",
      "version": 2,
      "versionNonce": 1973578350,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aE",
      "seed": 533615168,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 45,
      "y": 230,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 130,
      "height": 26,
      "text": "main",
      "fontSize": 16,
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
      "id": "2188d71e1ed2488186ce",
      "type": "text",
      "version": 2,
      "versionNonce": 2078423922,
      "isDeleted": false,
      "groupIds": [],
      "frameId": null,
      "index": "aF",
      "seed": 647323535,
      "strokeColor": "#334155",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "angle": 0,
      "x": 45,
      "y": 450,
      "roundness": null,
      "updated": 1793217138753,
      "link": null,
      "locked": false,
      "width": 130,
      "height": 26,
      "text": "feature",
      "fontSize": 16,
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
    "viewBackgroundColor": "#00000000"
  },
  "files": {},
  "previewHeight": 460,
  "previewZoom": 1
}
```



| | Merge | Rebase |
|---|---|---|
| History shape | Preserves true branch structure | Linear, tidy |
| Extra commit | Yes (merge commit) | No |
| Rewrites commits | No | Yes — new hashes |
| Safe on shared branches | Yes | **No** |
| Conflicts | Resolved once | May recur once per replayed commit |

## Commands
```bash
git switch feature
git rebase main                 # replay feature's commits on top of main

# keep a feature branch current with the remote main
git fetch origin
git rebase origin/main

git pull --rebase               # fetch + rebase instead of fetch + merge

# resolving a rebase conflict
git add <fixed-file>
git rebase --continue           # go on to the next commit
git rebase --skip               # drop the current commit entirely
git rebase --abort              # cancel; back to how things were before

# after rebasing a branch you had already pushed
git push --force-with-lease origin feature
```

## Interactive rebase: cleaning up your own commits
```bash
git rebase -i HEAD~4            # edit the last 4 commits
```
Git opens a todo list; change the word at the start of each line:

| Keyword | Effect |
|---|---|
| `pick` | keep the commit as-is |
| `reword` | keep it, edit the message |
| `squash` | meld into the previous commit, combining messages |
| `fixup` | meld into the previous commit, discard this message |
| `drop` | delete the commit |
| `edit` | pause so you can amend the commit's contents |

Typical use: before opening a PR, squash noisy "wip", "typo", "fix again" commits into a few meaningful ones. Reordering lines reorders commits.

## The Golden Rule
**Never rebase commits that have already been pushed to a shared branch.** Rebase rewrites history: C and D become C' and D' with *new hashes*. Anyone who already pulled the old commits now holds history that conflicts with yours, forcing a messy manual reconciliation.

- ✅ Safe: local branches only you use, or your own feature branch before anyone else has based work on it.
- ❌ Unsafe: `main`, `develop`, or any branch other people have pulled.

## `--force-with-lease` vs `--force`
After rebasing an already-pushed branch the histories diverge, so a normal push is rejected and you must force it. `--force-with-lease` refuses to overwrite the remote if someone else pushed commits you haven't fetched — a safety check plain `--force` lacks. Default to `--force-with-lease`.

## Rebase vs merge: which when?
- **Rebase** your private feature branch onto the latest `main` to stay current and keep history clean.
- **Merge** (often via a PR) when integrating into shared branches, or when you want an honest record of how work was combined.
- Many teams: rebase locally, then **squash-merge** or merge the PR on GitHub (see [[09 - Branching Strategies & PR Workflow|PR Workflow]]).

## Common mistakes
- Rebasing a shared/public branch — rewrites history others depend on.
- Using plain `--force` — can erase a teammate's pushed work without warning.
- Rebasing without confidence resolving conflicts — you may resolve the same conflict repeatedly, once per commit.
- Mixing up `ours`/`theirs` — reversed during a rebase (see [[03 - Merge Conflicts|Merge Conflicts]]).
- Panicking mid-rebase — `git rebase --abort` restores everything; and [[10 - Git Reflog, Cherry-pick & Recovery|reflog]] can recover even after a finished bad rebase.

## Related Concepts
- [[00 - Git & GitHub MOC|Git & GitHub MOC]] — overview and full topic index
- [[02 - Git Branching|Git Branching]]
- [[03 - Merge Conflicts|Merge Conflicts]]
- [[05 - Git Stash|Git Stash]] — stash uncommitted work before rebasing
- [[06 - Git Undoing Changes|Git Undoing Changes]] — the same "don't rewrite shared history" principle
- [[10 - Git Reflog, Cherry-pick & Recovery|Reflog & Recovery]]
