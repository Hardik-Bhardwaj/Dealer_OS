# Dealer Intelligence OS — Lava International

## Overview

Full-stack analytics command center for Lava International's 400+ distributor network. Solves "revenue bleeding" by tracking distributor health scores, K-Means tiers (Priority/Standard/Restricted), and surfacing AI-generated intervention memos.

## Architecture

- **Frontend**: React + Vite at `/` (dark mode, Lava Red brand)
- **API Server**: Express 5 at `/api`
- **Database**: PostgreSQL + Drizzle ORM
- **Charts**: Recharts (scatter plot, bar, line charts)
- **Animations**: Framer Motion

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

## Dashboard Pages

- `/` — Executive Command Center (KPI cards, tier breakdown, Colab integration guide)
- `/network` — Network Overview (Territory scatter plot, Revenue by Tier bar chart)
- `/distributors` — Distributor Deep-Dive (searchable table, 12-week trend chart)
- `/actions` — AI Action Center (restricted dealer list, intervention memos)

## Colab → Dashboard Integration

Push model results via:
```
POST /api/insights
Content-Type: application/json

{
  "summary": { "totalRevenueLocked": ..., "restrictedCount": ..., "avgHealthScore": ..., "recoverableWeeklyRevenue": ... },
  "distributors": [{ "name", "state", "district", "tier", "healthScore", ... }]
}
```

## API Endpoints

- `GET /api/distributors` — list with filters (state, district, tier, search)
- `GET /api/distributors/summary` — KPI aggregates
- `GET /api/distributors/tier-breakdown` — revenue/count by tier
- `GET /api/distributors/:id/trend` — 12-week health score trend
- `GET /api/distributors/:id/memo` — AI action memo for restricted dealer
- `GET /api/territories` — territory opportunity matrix data
- `POST /api/insights` — ingest Colab model output
- `GET /api/insights` — latest snapshot
- `POST /api/simulate` — generate 400+ synthetic distributor records

## Health Score Engine (Weights)

- Sell-through: 30%
- Payment delay: 25%
- Inventory age: 20%
- Return rate: 15%
- Dealer coverage: 10%

Tiers: Priority (70+), Standard (40–69), Restricted (<40)

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.
