---
starter_id: kotlin-compose-multiplatform
package_manager: gradle
project_name: zwierzatko-mobile
hints:
  language_family: multi
  team_size: solo
  deployment_target: android
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: best-effort
  path_taken: custom
  quality_override: false
  self_check_answers: null
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: false
  has_background_jobs: false
---

## Why this stack

Mobile jest dodatkiem do głównego webowego MVP i ma służyć równoległemu przećwiczeniu natywnego podejścia Android-first bez zwiększania ryzyka dla podstawowego produktu. Kotlin + Compose Multiplatform pozwala zacząć od aplikacji Android na Gradle i Jetpack Compose, a jednocześnie zostawia ścieżkę do współdzielenia części UI lub logiki z innymi platformami później. Ten plik jest świadomie zapisany obok głównego hand-offu: dokumentuje rekomendowany mobile stack, ale nie zastępuje `context/foundation/tech-stack.md`, który pozostaje wejściem dla bootstrappera web-app.
