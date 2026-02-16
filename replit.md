# replit.md

## Overview

This is an Arabic-language social deduction party game called "برا الحرجة" (Bra Al-Harja), similar to "Spyfall." Players join a game session where one player is secretly assigned as the spy while others receive a secret word from a chosen category. The spy must figure out the word while other players try to identify the spy through discussion and voting. The game is primarily client-side with minimal backend usage for optional analytics tracking.

The app is built as a full-stack TypeScript project with a React frontend and Express backend, designed as a mobile-first RTL (Right-to-Left) Arabic application with a dark neon visual theme.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend (client/)
- **Framework**: React with TypeScript, bundled by Vite
- **Routing**: Wouter (lightweight client-side router)
- **State Management**: React hooks + localStorage for player persistence, TanStack React Query for server state
- **UI Components**: shadcn/ui (new-york style) built on Radix UI primitives with Tailwind CSS
- **Animations**: Framer Motion for transitions and reveal animations
- **Effects**: canvas-confetti for celebration effects, custom sound manager for audio feedback
- **Styling**: Tailwind CSS with CSS variables for theming, dark neon color scheme (purple/cyan), Cairo font for Arabic text
- **Layout**: RTL (Right-to-Left) direction enforced globally for Arabic support, mobile-first max-width 448px container

### Game Logic (client/src/hooks/use-game-logic.ts)
- Entirely client-side state machine with phases: setup → category → pass → reveal → wait → vote → guess → end
- Word lists are hardcoded by category (Arabic words across 10 categories: food, TV shows, animals, clothes, anime, Spacetoon, professions, countries, K-pop, K-drama)
- Player names persisted in localStorage
- Spy is randomly assigned each round

### Backend (server/)
- **Framework**: Express.js on Node with TypeScript (tsx runner)
- **Purpose**: Minimal - serves static files and provides optional analytics endpoint
- **API**: Single POST `/api/sessions` endpoint to log game session starts
- **Database**: PostgreSQL via Drizzle ORM with `node-postgres` driver
- **Schema**: Single `game_sessions` table (id, started_at, player_count, category)
- **Storage Pattern**: Interface-based (`IStorage`) with `DatabaseStorage` implementation

### Shared Code (shared/)
- `schema.ts`: Drizzle table definitions and Zod validation schemas (via drizzle-zod)
- `routes.ts`: Typed API route definitions with Zod input/output schemas

### Build System
- **Dev**: Vite dev server with HMR proxied through Express
- **Production**: Vite builds client to `dist/public`, esbuild bundles server to `dist/index.cjs`
- **Path Aliases**: `@/` → `client/src/`, `@shared/` → `shared/`, `@assets/` → `attached_assets/`

### Database
- PostgreSQL required (DATABASE_URL environment variable)
- Drizzle ORM for schema definition and queries
- Drizzle Kit for migrations (`db:push` command)
- connect-pg-simple available for session storage if needed

## External Dependencies

- **PostgreSQL**: Primary database, connected via `DATABASE_URL` environment variable
- **Google Fonts**: Cairo font (Arabic), Architects Daughter, DM Sans, Fira Code loaded via CDN
- **Audio CDN**: Sound effects loaded from Pixabay CDN URLs (click, suspense, reveal, spy sounds)
- **Replit Plugins**: vite-plugin-runtime-error-modal, vite-plugin-cartographer, vite-plugin-dev-banner (dev only)
- **No authentication**: The app has no user auth system; it's a local multiplayer pass-and-play game