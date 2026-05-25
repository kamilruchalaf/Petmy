---
project: zwierzatko
researched_at: 2026-05-24T23:37:54+02:00
recommended_platform: Cloudflare Workers
runner_up: Netlify
context_type: mvp
tech_stack:
  language: TypeScript
  framework: Astro 6 + React 19 + Supabase
  runtime: Cloudflare Workers
---

## Recommendation

**Deploy on Cloudflare Workers.**

Cloudflare Workers is the best MVP deployment target for this project because the web app is already configured with Astro SSR, `@astrojs/cloudflare`, `wrangler`, `wrangler.jsonc`, `nodejs_compat`, and Workers observability. The interview answers favored stateless request/response, minimal monthly cost, global reach, and external managed services, which matches Workers Free/Paid pricing, global edge execution, and Supabase as the external data/auth provider.

## Platform Comparison

| Platform | CLI-first | Managed/Serverless | Agent-readable docs | Stable deploy API | MCP / Integration | Total |
|---|---|---|---|---|---|---|
| Cloudflare Workers | Pass | Pass | Pass | Pass | Pass | 5/5 |
| Netlify | Pass | Pass | Pass | Pass | Pass | 5/5 |
| Vercel | Pass | Pass | Pass | Pass | Partial | 4.5/5 |
| Fly.io | Pass | Partial | Pass | Pass | Fail | 3.5/5 |
| Railway | Pass | Partial | Partial | Pass | Partial | 3.5/5 |
| Render | Partial | Partial | Partial | Partial | Fail | 2/5 |

Cloudflare Workers: official Astro guidance supports SSR deployment with `wrangler deploy`, and the current project already has the Cloudflare adapter and Wrangler config. Workers Free is available by default, while Paid starts at a low monthly base and keeps bandwidth/egress simple for this MVP. Cloudflare docs expose agent-oriented markdown/llms surfaces and MCP servers across docs, Workers, and observability.

Netlify: strong CLI, deploy previews, rollback workflow, and official Netlify MCP. It is a credible alternative, but this project would need adapter/config migration away from Cloudflare, and Netlify's newer credit-based pricing is a weaker fit for the user's "minimal cost" preference.

Vercel: excellent CLI and rollback support, strong Astro docs, and Vercel MCP in beta. It ranks below Netlify for this project because it also requires adapter migration and is less aligned with the already-selected Cloudflare target.

Fly.io: strong CLI and persistent-process support, but this MVP does not need always-on processes. Fly has no current free account/free tier for new users, so it is penalized under the minimal-cost constraint.

Railway: fast full-stack DX and CLI-driven deployment, but its value is strongest when co-locating services. The user is fine with external Supabase, so Railway's integrated-service advantage matters less.

Render: simple managed hosting with free service previews for some workloads, but weaker agent integration and less direct Astro SSR + Cloudflare adapter fit make it a lower-ranked option.

### Shortlisted Platforms

#### 1. Cloudflare Workers (Recommended)

It wins because it matches the stack already in `web/`: Astro SSR, the Cloudflare adapter, Wrangler 4, and an existing `wrangler.jsonc`. It also matches the operating constraints: global users, stateless requests, low MVP cost, and external Supabase services.

#### 2. Netlify

Netlify is the best non-Cloudflare fallback because it is CLI-first, managed, well documented, and has an official MCP server. The gap is migration cost: the current app is already configured for Cloudflare Workers, not Netlify.

#### 3. Vercel

Vercel is a strong third option because its CLI and rollback flow are mature and Astro is supported. It loses here because Vercel MCP is still beta and the project would need adapter migration without gaining much for this specific MVP.

## Anti-Bias Cross-Check: Cloudflare Workers

### Devil's Advocate - Weaknesses

1. Astro SSR on Workers runs on `workerd`, not a full Node server. `nodejs_compat` helps, but future dependencies may still assume unsupported Node APIs.
2. Supabase remains an external provider, so database latency, auth configuration, migrations, backups, and data rollback are outside Cloudflare's deployment rollback.
3. `wrangler rollback` can restore a Worker deployment, but it cannot undo Supabase schema or data changes.
4. Runtime debugging is different from Node hosting; useful logs require Workers observability and `wrangler tail` habits from day one.
5. Preview or local deployment can accidentally use production Supabase secrets unless environments are split explicitly.

### Pre-Mortem - How This Could Fail

Six months after launch, the Cloudflare decision fails because the project treats Workers like ordinary Node hosting. A later feature adds a package that depends on Node APIs not fully covered by `nodejs_compat`; local development looks fine, but production deploys start failing or behaving differently under `workerd`. At the same time, preview deploys reuse production Supabase credentials, so test users and fake pet records appear in the live database. The team rolls back the Worker quickly, but the rollback does not undo data writes or any Supabase migration. Debugging takes longer than expected because logs were never made part of the deployment routine, and the agent has to rediscover Wrangler commands under pressure. The cost and global latency assumptions were right; the failure came from runtime compatibility, environment separation, and treating application rollback and data rollback as the same thing.

### Unknown Unknowns

- The correct deployment path for this Astro SSR setup is Workers-oriented `wrangler deploy`; do not substitute a generic Pages command without re-checking the adapter output.
- `nodejs_compat` is a compatibility layer, not a full guarantee that every Node package will run unchanged on Workers.
- Workers secrets and local `.env` / `.dev.vars` behavior differ; production secrets must be created through Wrangler or the dashboard, not committed.
- Worker rollback is fast, but Supabase migrations and data writes need their own rollback playbook.
- Free/low-cost plans still have usage limits; add alerts before a public launch or shared demo.

## Operational Story

- **Preview deploys**: Use Cloudflare Workers deploys from branches or CI once GitHub integration is configured; protect non-production previews with separate Supabase projects or separate credentials before inviting testers.
- **Secrets**: Store `SUPABASE_URL` and `SUPABASE_KEY` as Workers secrets with `npx wrangler secret put`; keep `.env` and `.dev.vars` local-only and gitignored.
- **Rollback**: Use Cloudflare's deployment rollback flow or `npx wrangler rollback` for Worker code. Treat Supabase schema/data rollback as a separate manual gate.
- **Approval**: An agent may run lint/build and prepare a deploy plan. A human approves first production deploy, custom domain changes, secret rotation, Supabase migration, and any destructive data action.
- **Logs**: Use `npx wrangler tail 10x-astro-starter` for runtime logs and Cloudflare Workers observability for production inspection.

## Risk Register

| Risk | Source | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| Future dependency assumes unsupported Node APIs | Devil's advocate | M | H | Run `npm run build` and a staging `npx wrangler deploy` after dependency changes; reject packages that fail under Workers. |
| Preview uses production Supabase credentials | Pre-mortem | M | H | Create separate Supabase credentials for preview/staging before enabling public previews. |
| Worker rollback does not roll back data | Unknown unknowns | M | H | Require a migration rollback note for every Supabase schema/data change. |
| Runtime logs are missing during incident response | Devil's advocate | M | M | Keep `observability.enabled` in `wrangler.jsonc` and document `npx wrangler tail 10x-astro-starter`. |
| Cost assumptions drift after public sharing | Research finding | L | M | Add Cloudflare usage alerts and Supabase usage alerts before broader launch. |
| Wrong deploy command copied from generic Pages docs | Unknown unknowns | M | M | Use Astro Workers docs and the local `wrangler.jsonc`; prefer `npx wrangler deploy` for this SSR config. |

## Getting Started

1. From `web/`, run `npm install`, then `npm run lint` and `npm run build`.
2. Authenticate Wrangler with `npx wrangler login`.
3. Create production secrets with `npx wrangler secret put SUPABASE_URL` and `npx wrangler secret put SUPABASE_KEY`.
4. Deploy the Astro SSR Worker with `npx wrangler deploy` from `web/`.
5. Verify runtime behavior with the deployed URL and inspect logs with `npx wrangler tail 10x-astro-starter`.

## Sources Checked

- Cloudflare Astro Workers guide: https://developers.cloudflare.com/workers/frameworks/framework-guides/astro/
- Cloudflare Workers pricing: https://developers.cloudflare.com/workers/platform/pricing/
- Cloudflare Workers secrets: https://developers.cloudflare.com/workers/configuration/secrets/
- Cloudflare Wrangler Workers commands: https://developers.cloudflare.com/workers/wrangler/commands/workers/
- Vercel CLI and rollback docs: https://vercel.com/docs/cli and https://vercel.com/docs/cli/rollback
- Netlify deploy and MCP docs: https://docs.netlify.com/deploy/deploy-overview/ and https://docs.netlify.com/build/build-with-ai/netlify-mcp-server/
- Fly.io CLI and pricing docs: https://fly.io/docs/flyctl/ and https://fly.io/docs/about/pricing/

## Out of Scope

The following were not evaluated in this research:

- Docker image configuration
- CI/CD pipeline setup
- Production-scale architecture (multi-region, HA, DR)
