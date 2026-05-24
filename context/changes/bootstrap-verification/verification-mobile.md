---
bootstrapped_at: 2026-05-24T20:04:35Z
starter_id: kotlin-compose-multiplatform
starter_name: "Kotlin + Compose Multiplatform"
project_name: zwierzatko-mobile
language_family: multi
package_manager: gradle
cwd_strategy: git-clone
bootstrapper_confidence: best-effort
phase_3_status: ok
audit_command: "null"
target_directory: multiplatform
---

## Hand-off

```yaml
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
```

### Why this stack

Mobile jest dodatkiem do głównego webowego MVP i ma służyć równoległemu przećwiczeniu natywnego podejścia Android-first bez zwiększania ryzyka dla podstawowego produktu. Kotlin + Compose Multiplatform pozwala zacząć od aplikacji Android na Gradle i Jetpack Compose, a jednocześnie zostawia ścieżkę do współdzielenia części UI lub logiki z innymi platformami później. Ten plik jest świadomie zapisany obok głównego hand-offu: dokumentuje rekomendowany mobile stack, ale nie zastępuje `context/foundation/tech-stack.md`, który pozostaje wejściem dla bootstrappera web-app.

## Pre-scaffold verification

| Signal | Value | Severity | Notes |
| --- | --- | --- | --- |
| npm package | not run | n/a | non-JS starter; no npm create package |
| GitHub repo | `JetBrains/compose-multiplatform` last pushed 2026-05-23T03:48:57Z | fresh | from card `docs_url` |

## Scaffold log

**Resolved invocation**: `git clone https://github.com/JetBrains/compose-multiplatform-template .bootstrap-scaffold`
**Strategy**: git-clone
**Exit code**: 0
**Target directory**: `multiplatform/`
**Files moved**: 16 top-level paths moved
**Conflicts (.scaffold siblings)**: none
**.gitignore handling**: moved inside `multiplatform/`
**.bootstrap-scaffold cleanup**: renamed to `multiplatform/`; upstream `.git/` deleted

Moved paths:

- `.gitignore`
- `.run/`
- `LICENSE.txt`
- `README.md`
- `androidApp/`
- `build.gradle.kts`
- `cleanup.sh`
- `desktopApp/`
- `gradle/`
- `gradle.properties`
- `gradlew`
- `gradlew.bat`
- `iosApp/`
- `readme_images/`
- `settings.gradle.kts`
- `shared/`

## Post-scaffold audit

**Tool**: skipped - no built-in audit tool for multi
**Recommended external tool**: no single audit tool covers this multi-language stack. For Gradle/Kotlin projects, configure a dedicated dependency scanner such as OWASP Dependency-Check, Snyk, or GitHub Dependabot.

## Additional verification

`./gradlew tasks` was run from `multiplatform/` and completed successfully. Gradle downloaded `gradle-8.2.1-bin.zip` and Kotlin/Native `1.9.21` on first run. The build reported warnings about unused source-set variables and Kotlin default hierarchy template configuration, but the task list resolved successfully.

## Hints recorded but not acted on

| Hint | Value |
| --- | --- |
| bootstrapper_confidence | best-effort |
| quality_override | false |
| path_taken | custom |
| self_check_answers | null |
| team_size | solo |
| deployment_target | android |
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

- Open `multiplatform/` in IntelliJ IDEA or Android Studio with a JDK 17+ Gradle runtime.
- Review the archived template status and consider regenerating with the official JetBrains KMP wizard if you need the newest project layout.
- Configure a Gradle/Kotlin dependency scanner separately; bootstrapper v1 does not include a built-in audit for `multi`.
