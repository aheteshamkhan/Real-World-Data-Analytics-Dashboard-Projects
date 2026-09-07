# Power BI Analytics Dashboard

**Interactive Business Intelligence & Decision Analytics Platform**

A locally runnable Business Intelligence analytics platform demonstrating SQL extraction, star-schema data modeling, DAX-inspired calculations and interactive dashboard visualization — built with React, TypeScript, Vite, Tailwind CSS and Recharts.

> ⚠️ **Portfolio project.** This application uses a synthetic dataset and a locally simulated pipeline. It is not an official Microsoft Power BI product and does not connect to any Power BI service.

---

## Overview

The dashboard answers real business questions:

- How much revenue are we generating, and how does it compare with the previous period?
- Which products, categories, regions and channels perform best — and which are underperforming?
- What trends should management pay attention to?
- Where does profit come from, and is margin healthy?

Every number on screen is **computed live** from a bundled synthetic dataset (`src/data/*.json`, ~4,200 orders across 2023–2024). There are no hardcoded figures, no backend, and no network calls.

## Features

- **Interactive KPI dashboard** — 5 KPI cards (Revenue, Profit, Profit Margin, Orders, AOV) with period-over-period deltas and sparklines
- **Dynamic cross-filtering** — date range, region, category and channel slicers that update every chart, KPI and table instantly
- **Revenue analytics** — monthly revenue/profit trend, category and channel distributions
- **Profitability analysis** — revenue vs cost vs profit grouped by category
- **Product performance** — ranked top-products table with margins and concentration
- **Drill-through product analytics** — click any product for its full P&L, monthly trend, regional performance, top customers and ranking
- **Data model visualization** — interactive star-schema diagram (FACT_SALES + 4 dimensions)
- **Pipeline visualization** — 5-stage ETL view with live pipeline status and refresh simulation
- **Business insights** — auto-derived insight cards with impact ratings, regenerated from the current filters
- **Global search** — products, customers, regions and order IDs
- **CSV export** — downloads the currently filtered dataset as a real CSV file
- **Notifications** — alerts derived from the dataset
- **Empty states, error handling, keyboard navigation** — professional UX throughout

## Tech Stack

| Layer | Technology |
| --- | --- |
| Frontend | React 18 + TypeScript |
| Build | Vite 5 |
| Styling | Tailwind CSS 3 |
| Charts | Recharts |
| Icons | Lucide React |
| Routing | React Router (HashRouter) |
| Typography | Inter Variable (self-hosted via Fontsource) |
| Data | Local JSON datasets (`src/data/*.json`) |

## Architecture

```
RAW DATA  →  TRANSFORMATION  →  STAR SCHEMA  →  ANALYTICS MEASURES  →  VISUALIZATION  →  BUSINESS INSIGHTS
   JSON           join + clean      FACT_SALES       DAX-inspired           charts, KPIs,      insight cards,
(5 sources)       in loader        + 4 dims          TS functions           tables, filters    notifications
```

- **Data layer** — `scripts/generate-data.mjs` deterministically generates the synthetic dataset; `src/data/index.ts` loads and joins it into star-schema-shaped rows.
- **Measure layer** — `src/utils/calculations.ts` implements DAX-inspired measures (`Total Revenue`, `Profit Margin`, `Revenue Growth`, …) as pure TypeScript functions.
- **State layer** — `src/store/AppContext.tsx` holds the global filter state and refresh status; every view consumes the same context.
- **View layer** — pages (`src/pages`) composed of reusable chart, KPI and UI components (`src/components`).

## Local Setup

Requires Node.js 18+.

```bash
npm install
npm run dev
```

Open http://localhost:5173 — the app runs 100% locally.

Other scripts:

```bash
npm run build        # production build (dist/)
npm run preview      # serve the production build
npm run typecheck    # TypeScript check
npm run generate:data  # regenerate the synthetic dataset (deterministic)
```

## Analytics

Core measures (all implemented in `src/utils/calculations.ts`, documented in `docs/dax-measures.md`):

- **Total Revenue** — `SUM(Sales[Revenue])`
- **Total Profit** — `SUM(Sales[Profit])`
- **Profit Margin** — `DIVIDE([Total Profit], [Total Revenue])`
- **Total Orders** — `DISTINCTCOUNT(Sales[Order ID])`
- **Total Units** — `SUM(Sales[Quantity])`
- **Average Order Value** — `DIVIDE([Total Revenue], [Total Orders])`
- **Revenue / Profit Growth** — `(Current − Previous) / Previous` vs the same window one year earlier
- **Rankings** — top products, regions, categories, channels and customers

## Project Structure

```
├── database/            # SQL schema for the star schema (reference)
├── docs/                # data-model.md, dax-measures.md, project-overview.md
├── scripts/             # deterministic dataset generator
├── src/
│   ├── components/      # charts, dashboard, filters, layout, ui
│   ├── data/            # generated JSON datasets
│   ├── pages/           # Dashboard, ProductDetail, DataModel, Pipeline, Insights, Settings
│   ├── store/           # global filter + data-status context
│   ├── types/           # analytics domain types
│   └── utils/           # calculations, filters, insights, formatters, export
└── README.md
```

## Disclaimer

This project uses a synthetic dataset and locally simulated pipeline behavior for portfolio demonstration purposes. Metrics such as "15+ analytical views", "8+ modeled data sources" and "40% faster simulated reporting workflow" are portfolio/demo descriptors, not verified real-company production results. The DAX-style measures are conceptually inspired by Power BI but are executed locally in TypeScript — the web app itself is not running Power BI.

---

Built as a portfolio project for **Business Intelligence / Data Analytics**.