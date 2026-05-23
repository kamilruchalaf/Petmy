---
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
---

## Why this stack

Zwierzątko ma krótki, trzytygodniowy MVP budowany po godzinach, a główny zakres web-app obejmuje logowanie, prywatne dane właściciela, profil zwierzęcia i trwałą historię zdrowia. 10x Astro Starter daje Astro, React, TypeScript, Supabase i Cloudflare w jednym spójnym zestawie: auth i baza są dostępne od początku, TypeScript oraz schematy pomagają agentowi utrzymać kontrakty, a Cloudflare Pages jest domyślnym celem wdrożenia. Mobile traktujemy jako osobny równoległy projekt ćwiczebny, więc główny hand-off pozostaje skupiony na web-app i może być bezpośrednio użyty przez bootstrapper.
