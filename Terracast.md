```
---
aliases:
  - TerraCast
  - Meteon Labs TerraCast
tags:
  - terracast
  - meteon-labs
  - project-hub
---

# TerraCast — Project Hub

TerraCast is Meteon Labs' hyperlocal, AI-assisted weather intelligence platform for India. This note is the central navigation point for the project and creates the main hub in Obsidian Graph View.

## Core Documentation

| Note | Purpose |
|---|---|
| [[TerraCast Project Overview]] | Original project vision, purpose and product relationships |
| [[PRD|TerraCast PRD]] | Product requirements and backend development specification |
| [[Architecture]] | System boundaries, approved stack, data flow, APIs and folder structure |
| [[Rules]] | Non-negotiable engineering, security, localization and quality rules |
| [[Phases]] | Sequential v1 implementation plan and exit criteria |
| [[Design]] | Professional SaaS visual system, responsive UX and accessibility requirements |

## Recommended Reading Order

1. Start with [[TerraCast Project Overview]] for the original project vision.
2. Read [[PRD]] for the complete product and backend requirements.
3. Use [[Architecture]] to understand how the system fits together.
4. Read [[Rules]] before making any implementation decision.
5. Use [[Phases]] to identify the currently permitted work.
6. Apply [[Design]] while building and reviewing every user-facing screen.

## Current Product Direction

- Public SaaS website plus authenticated weather application
- Next.js, TypeScript and Tailwind frontend
- Django and Django REST Framework backend
- Supabase Auth and PostgreSQL
- CrewAI for controlled brief generation and language adaptation
- Regional-language and literacy-independent presentation
- Verified weather alerts whose authority does not depend on AI
- Redis, Docker, paid plans, reports, APIs and MCP deferred beyond v1

## Delivery Flow

[[Phases#Phase 1 — Product Foundation and Authentication|Phase 1: Foundation and Auth]] → [[Phases#Phase 2 — Core Weather Data Pipeline|Phase 2: Weather Data]] → [[Phases#Phase 3 — AI Weather Brief|Phase 3: AI Brief]] → [[Phases#Phase 4 — Regional Language Experience|Phase 4: Languages]] → [[Phases#Phase 5 — Mode Personalization and Literacy-Independent Presentation|Phase 5: Modes and Animations]] → [[Phases#Phase 6 — Verified Alerts|Phase 6: Alerts]] → [[Phases#Phase 7 — Quality, Deployment and v1 Release|Phase 7: Release]]

## Graph View

Open Obsidian's Graph View after placing this entire `docs` folder in the same vault as `PRD.md` and `TerraCast Project Overview.md`. The original project, PRD, documentation hub and four technical notes will appear as one connected graph. Search or filter by `tag:#terracast` to focus the graph on this project.
```