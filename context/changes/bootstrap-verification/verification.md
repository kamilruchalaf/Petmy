---
bootstrapped_at: 2026-05-24T19:46:55Z
starter_id: 10x-astro-starter
starter_name: "10x Astro Starter (Astro + Supabase + Cloudflare)"
project_name: zwierzatko
language_family: js
package_manager: npm
cwd_strategy: git-clone
bootstrapper_confidence: first-class
phase_3_status: ok
audit_command: "npm audit --json"
---

## Hand-off

```yaml
starter_id: 10x-astro-starter
package_manager: npm
project_name: zwierzatko
hints:
  language_family: js
  team_size: solo
  deployment_target: cloudflare-pages
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: first-class
  path_taken: standard
  quality_override: false
  self_check_answers: null
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: false
  has_background_jobs: false
```

### Why this stack

Zwierzątko ma krótki, trzytygodniowy MVP budowany po godzinach, a główny zakres web-app obejmuje logowanie, prywatne dane właściciela, profil zwierzęcia i trwałą historię zdrowia. 10x Astro Starter daje Astro, React, TypeScript, Supabase i Cloudflare w jednym spójnym zestawie: auth i baza są dostępne od początku, TypeScript oraz schematy pomagają agentowi utrzymać kontrakty, a Cloudflare Pages jest domyślnym celem wdrożenia. Mobile traktujemy jako osobny równoległy projekt ćwiczebny, więc główny hand-off pozostaje skupiony na web-app i może być bezpośrednio użyty przez bootstrapper.

## Pre-scaffold verification

| Signal | Value | Severity | Notes |
| --- | --- | --- | --- |
| npm package | not run | n/a | skipped because the starter command starts with `git clone` |
| GitHub repo | `przeprogramowani/10x-astro-starter` last pushed 2026-05-17T10:33:39Z | fresh | from card `docs_url` |

## Scaffold log

**Resolved invocation**: `git clone https://github.com/przeprogramowani/10x-astro-starter .bootstrap-scaffold && cd .bootstrap-scaffold && npm install`
**Strategy**: git-clone
**Exit code**: 0
**Files moved**: 20 top-level paths moved
**Conflicts (.scaffold siblings)**: none
**.gitignore handling**: moved silently
**.bootstrap-scaffold cleanup**: deleted

Moved paths:

- `.env.example`
- `.github/`
- `.gitignore`
- `.husky/`
- `.nvmrc`
- `.prettierrc.json`
- `.vscode/`
- `CLAUDE.md`
- `README.md`
- `astro.config.mjs`
- `components.json`
- `eslint.config.js`
- `node_modules/`
- `package-lock.json`
- `package.json`
- `public/`
- `src/`
- `supabase/`
- `tsconfig.json`
- `wrangler.jsonc`

## Post-scaffold audit

**Tool**: `npm audit --json`
**Summary**: 0 CRITICAL, 1 HIGH, 9 MODERATE, 0 LOW
**Direct vs transitive**: 0/0/2/0 direct of total 0/1/9/0

#### CRITICAL findings

None.

#### HIGH findings

- `devalue` (`5.6.3 - 5.8.0`) — GHSA-77vg-94rm-hx3p, "Svelte devalue: DoS via sparse array deserialization". Transitive. Fix available.

#### MODERATE findings

- `@astrojs/check` (`>=0.9.3`) — direct dependency affected via `@astrojs/language-server`. Fix available as `@astrojs/check@0.9.2`, marked semver-major by npm.
- `@astrojs/language-server` (`>=2.14.0`) — transitive via `volar-service-yaml`.
- `@cloudflare/vite-plugin` (`<=0.0.0-fff677e35 || 0.0.7 - 1.37.2`) — transitive via `miniflare`, `wrangler`, and `ws`.
- `miniflare` (`<=0.0.0-fff677e35 || 3.20250204.0 - 4.20260518.0`) — transitive via `ws`.
- `volar-service-yaml` (`<=0.0.70`) — transitive via `yaml-language-server`.
- `wrangler` (`<=0.0.0-kickoff-demo || 3.108.0 - 4.93.0`) — direct dependency affected via `miniflare`.
- `ws` (`8.0.0 - 8.20.0`) — GHSA-58qx-3vcg-4xpx, "ws: Uninitialized memory disclosure". Transitive. Fix available.
- `yaml` (`2.0.0 - 2.8.2`) — GHSA-48c2-rrv3-qjmp, "yaml is vulnerable to Stack Overflow via deeply nested YAML collections". Transitive.
- `yaml-language-server` (`1.11.1-08d5f7b.0 - 1.21.1-f1f5a94.0 || 1.22.1-0ae5603.0 - 1.22.1-fc5f874.0`) — transitive via `yaml`.

#### LOW / INFO findings

None.

## Hints recorded but not acted on

| Hint | Value |
| --- | --- |
| bootstrapper_confidence | first-class |
| quality_override | false |
| path_taken | standard |
| self_check_answers | null |
| team_size | solo |
| deployment_target | cloudflare-pages |
| ci_provider | github-actions |
| ci_default_flow | auto-deploy-on-merge |
| has_auth | true |
| has_payments | false |
| has_realtime | false |
| has_ai | false |
| has_background_jobs | false |

## Next steps

Next: a future skill will set up agent context (CLAUDE.md, AGENTS.md). For now, your project is scaffolded and verified - happy hacking.

Useful manual steps in the meantime:

- Review any `.scaffold` siblings the conflict policy created and decide which version of each file to keep.
- Address audit findings per your project's risk tolerance - the full breakdown is in this log.
