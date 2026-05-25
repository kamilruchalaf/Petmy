# Deployment Plan

## Summary

Deploy the `web/` Astro SSR application to Cloudflare Workers. Production deployments are triggered by Cloudflare Workers Builds on every push to the `master` branch, not by GitHub Actions.

The first production deployment uses a hosted Supabase project. `SUPABASE_URL` and `SUPABASE_KEY` are configured as Cloudflare Worker secrets before the production smoke test.

## Phase Checklist

- [ ] Phase 1: Prepare hosted Supabase production project.
- [ ] Phase 2: Verify the local web build.
- [ ] Phase 3: Create or connect the Cloudflare Worker.
- [ ] Phase 4: Configure Cloudflare Workers Builds from GitHub.
- [ ] Phase 5: Add production runtime secrets.
- [ ] Phase 6: Run the first production deployment.
- [ ] Phase 7: Verify automatic deployment from `master`.
- [ ] Phase 8: Complete production smoke tests and record rollback notes.

## Phase 1: Prepare Supabase

- [ ] Create or select the hosted production Supabase project.
- [ ] Copy the production Project URL.
- [ ] Copy the production anon key.
- [ ] Configure Supabase Auth Site URL for the deployed Worker URL.
- [ ] Add production redirect URLs for the deployed Worker URL.

## Phase 2: Verify Local Build

Run all commands from `web/`.

```bash
cd web
nvm use
npm ci
npm run lint
npm run build
```

Acceptance:

- [ ] `npm run lint` passes.
- [ ] `npm run build` passes.
- [ ] The build output confirms `output: "server"` and adapter `@astrojs/cloudflare`.

## Phase 3: Create Or Connect Worker

- [ ] Create or identify the Cloudflare Worker named `10x-astro-starter`.
- [ ] Confirm the Worker name matches `web/wrangler.jsonc`.
- [ ] Confirm `nodejs_compat` and observability remain enabled in `web/wrangler.jsonc`.

## Phase 4: Configure Cloudflare Builds

Configure Cloudflare Workers Builds / Git integration:

- [ ] Git provider: GitHub.
- [ ] Repository: this Petmy repository.
- [ ] Root directory: `web`.
- [ ] Production branch: `master`.
- [ ] Build command: `npm run build`.
- [ ] Deploy command: `npx wrangler deploy`.
- [ ] Non-production branch builds: disabled for the first deployment.

GitHub Actions must not deploy the Worker. It may remain as CI for lint and build only.

## Phase 5: Add Runtime Secrets

Add these as Cloudflare Worker secrets:

- [ ] `SUPABASE_URL`
- [ ] `SUPABASE_KEY`

CLI option from `web/`:

```bash
npx wrangler secret put SUPABASE_URL
npx wrangler secret put SUPABASE_KEY
npx wrangler secret list
```

Do not print secret values in shell history, logs, commits, or the deployment plan.

## Phase 6: First Production Deployment

- [ ] Run the first deployment through Cloudflare with Save and Deploy, retry build, or the configured Workers Build trigger.
- [ ] Confirm the Cloudflare build succeeds.
- [ ] Confirm the active Worker deployment changes.
- [ ] Record the deployed `workers.dev` URL.

## Phase 7: Verify Auto-Deploy From Master

- [ ] Push a real change or controlled test commit to `master`.
- [ ] Confirm Cloudflare starts a new Workers Build.
- [ ] Confirm the new build deploys to production.
- [ ] Confirm GitHub Actions did not run any Cloudflare deploy step.

## Phase 8: Production Smoke Test

- [ ] Open the deployed URL.
- [ ] Verify `/` loads.
- [ ] Verify `/dashboard` redirects unauthenticated users to sign-in.
- [ ] Verify sign-up/sign-in with a test user.
- [ ] Check live logs:

```bash
npx wrangler tail 10x-astro-starter
```

- [ ] Confirm no runtime errors appear during smoke testing.

## Rollback Notes

- Application rollback: use Cloudflare Worker rollback or `npx wrangler rollback`.
- Data rollback: handle Supabase schema and data separately.
- Do not treat Worker rollback as a rollback for Supabase data or migrations.

## Assumptions

- The first deployment uses the `workers.dev` URL, not a custom domain.
- Preview deployments for non-`master` branches are disabled initially.
- GitHub Actions remains CI-only unless explicitly changed later.
