# Repository Guidelines

This directory is the Astro 6 SSR web app with React 19 islands, Supabase auth, Tailwind 4, shadcn/ui, and Cloudflare Workers deployment. See `@../AGENTS.md` for repo-wide rules.

## Local Rules First

- Run commands from `web/`; this directory owns `@package.json`, npm scripts, `.nvmrc`, ESLint, Prettier, Wrangler, and Supabase config.
- Keep environment values in `.env` for Node/Astro and `.dev.vars` for Cloudflare local dev. `SUPABASE_URL` and `SUPABASE_KEY` are server-only secrets in `@astro.config.mjs`.
- Use the `@/*` alias for imports from `src/`, as defined in `@tsconfig.json`.
- Prefer Astro components for static pages and layouts. Add React components only for interactive UI; do not add Next.js directives.

## Commands

- `npm run dev` starts the Astro dev server.
- `npm run build` runs the production Cloudflare SSR build.
- `npm run lint` runs ESLint with strict TypeScript, React, Astro, accessibility, and Prettier rules.
- `npm run format` runs Prettier with Astro and Tailwind plugins.

## File Layout & Naming

Place pages in `src/pages/`, API endpoints in `src/pages/api/`, reusable UI in `src/components/`, auth form pieces in `src/components/auth/`, and helpers in `src/lib/`. Keep shadcn/ui primitives in `src/components/ui/`; use `cn()` from `@/lib/utils` for conditional Tailwind classes.

## Auth & Backend

Route protection is centralized in `@src/middleware.ts`; update `PROTECTED_ROUTES` there. Supabase SSR client creation belongs in `@src/lib/supabase.ts`. Auth actions use Astro API routes under `@src/pages/api/auth/`.

## Testing

No test runner is configured yet. Before handing off web changes, run `npm run lint` and `npm run build`.
