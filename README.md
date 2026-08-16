---
tags: [vault-home, second-brain, obsidian]
aliases: [SecondBrain Home, Vault Home]
updated: 2026-08-16
---

# SecondBrain — Vault Home

This vault is the central workspace for computer science notes, placement prep, DSA, system design, and web-development learning.

## 🧭 Vault Navigation Map

```mermaid
flowchart TD
    Home["SecondBrain Home"] --> CS["Computer Science"]
    CS --> DSA["DSA"]
    CS --> Web["Web Development"]
    CS --> Sys["System Design"]
    Web --> JS["JavaScript"]
    Web --> React["React"]
    Web --> APIs["APIs"]
    Web --> Git["Git"]

    DSA --> Patterns["Patterns + Problems"]
    JS --> Runtime["Runtime + Interview Traces"]
    React --> UI["UI Architecture + Hooks"]
    Sys --> Scale["Scalable Systems"]

    classDef home fill:#EDE9FE,stroke:#7C3AED,color:#111827,stroke-width:2px
    classDef area fill:#DBEAFE,stroke:#2563EB,color:#111827,stroke-width:2px
    classDef topic fill:#DCFCE7,stroke:#16A34A,color:#111827,stroke-width:2px
    classDef detail fill:#FEF3C7,stroke:#D97706,color:#111827,stroke-width:2px
    class Home home
    class CS area
    class DSA,Web,Sys topic
    class JS,React,APIs,Git,Patterns,Runtime,UI,Scale detail
```

## 📌 Primary Maps of Content

- [[Computer Science/System Design/System Design MOC|System Design MOC]]
- [[Computer Science/Web Development/JavaScript/JavaScript MOC|JavaScript MOC]]
- [[Computer Science/Web Development/React/00 - React MOC|React MOC]]
- [[Computer Science/Web Development/APIs/API|API Master Guide]]

## 🔌 Connected Tooling

```mermaid
flowchart TD
    Vault["Obsidian Vault"] --> MCP["MCP Plugins"]
    MCP --> Claude["Claude"]
    MCP --> Gemini["Gemini"]
    MCP --> Antigravity["Antigravity CLI"]
    Vault --> Diagrams["Mermaid / Diagram Plugins"]
    Vault --> Query["Dataview"]
    Vault --> Format["Linter + Table Editor"]

    classDef vault fill:#EDE9FE,stroke:#7C3AED,color:#111827,stroke-width:2px
    classDef tool fill:#DBEAFE,stroke:#2563EB,color:#111827,stroke-width:2px
    classDef ai fill:#FEF3C7,stroke:#D97706,color:#111827,stroke-width:2px
    class Vault vault
    class MCP,Diagrams,Query,Format tool
    class Claude,Gemini,Antigravity ai
```

## Recently Updated Notes

```dataview
TABLE file.mtime AS "Modified", tags AS "Tags"
FROM "Computer Science"
SORT file.mtime DESC
LIMIT 12
```
