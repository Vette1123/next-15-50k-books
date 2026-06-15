# 📚 next-15-50k-books

> Browse a 50,000-book library with instant search and author filtering — Next.js 15, React 19, Drizzle, and Neon Postgres.

![Next.js](https://img.shields.io/badge/Next.js-15_canary-000000?logo=nextdotjs)
![React](https://img.shields.io/badge/React-19_RC-61dafb?logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript)
![Drizzle](https://img.shields.io/badge/Drizzle-ORM-c5f74f)
![Neon](https://img.shields.io/badge/Neon-Postgres-00e599)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-3-38bdf8?logo=tailwindcss)

### 🚀 [Try it live →](https://next-15-50k-books.vercel.app)

A book-inventory playground for the bleeding edge of the React/Next.js stack. The dataset (`scripts/books.csv`, ~50k rows) is loaded into Neon Postgres, then served through React Server Components with streaming Suspense. Search stays snappy thanks to a `pg_trgm` GIN index and Postgres `unaccent`, while a custom backpressure hook keeps the UI smooth as you type.

## ✨ Features

- **Streaming RSC pagination** — server components fetch, paginate, and stream results (30 per page) via Suspense.
- **Accent-insensitive search** — `lower(unaccent(title)) LIKE …` powered by a Postgres trigram GIN index.
- **Author filter sidebar** — multi-select authors via URL search params; works with native `<Form>` (Next 15) for progressive enhancement.
- **Backpressure-aware input** — `useBackpressure` + React 19's `use()` keep typing responsive while older requests are discarded.
- **Edge-friendly data layer** — `@neondatabase/serverless` + Drizzle ORM over HTTP, no connection pool to babysit.
- **Polished UI** — Tailwind, Radix primitives (shadcn-style), Geist font, Sonner toasts, and a welcome toast on first visit.

## 🛠️ Tech Stack

| Layer | Technology |
| --- | --- |
| Framework | Next.js 15 (canary, App Router, Turbopack dev) |
| UI | React 19 RC, Tailwind CSS, Radix UI, Lucide icons |
| Data | Drizzle ORM, `@neondatabase/serverless`, `@vercel/postgres` (seeder) |
| Database | Neon / Vercel Postgres with `pg_trgm` + `unaccent` |
| Tooling | TypeScript 5, Prettier, Bun (seed script) |

## 📖 Getting Started

```bash
pnpm install
# Provision the Postgres extensions (run once against your DB):
#   CREATE EXTENSION IF NOT EXISTS unaccent;
#   CREATE EXTENSION IF NOT EXISTS pg_trgm;
pnpm seed     # uses Bun to import scripts/books.csv into the books table
pnpm dev      # http://localhost:3000
```

### Environment variables

| Variable | Description |
| --- | --- |
| `POSTGRES_URL` | Neon / Vercel Postgres connection string used at runtime by Drizzle. |
| `POSTGRES_URL_NON_POOLING` | Direct (non-pooled) connection used by `@vercel/postgres` during `pnpm seed`. |

The seed script relies on Bun (`bun run ./scripts/seed.mjs`); install it from [bun.sh](https://bun.sh) if you don't already have it.

## 🙌 Credits

Built by [Mohamed Gado](https://mohamedgado.com).
