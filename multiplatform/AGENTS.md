# Repository Guidelines

This directory is the Kotlin Compose Multiplatform app. It is separate from the web implementation; see `@../AGENTS.md` for repo-wide naming and planning-file rules.

## Local Rules First

- Run Gradle commands from `multiplatform/` using the checked-in wrapper.
- Keep shared UI and cross-platform contracts in `shared/src/commonMain/`. Platform-specific implementations belong in `shared/src/androidMain/`, `shared/src/desktopMain/`, or `shared/src/iosMain/`.
- `androidApp/` owns the Android application shell and package metadata. `desktopApp/` owns the JVM desktop entry point and native distribution settings. `iosApp/` is the Xcode wrapper around the shared framework.
- Keep Kotlin style aligned with `kotlin.code.style=official` in `@gradle.properties`; the configured JVM toolchain is 17.

## Commands

- `./gradlew build` builds all configured modules.
- `./gradlew :androidApp:assembleDebug` builds the Android debug app.
- `./gradlew :desktopApp:run` runs the desktop target.

## File Layout & Naming

The Gradle modules are declared in `@settings.gradle.kts`: `:shared`, `:androidApp`, and `:desktopApp`. Add shared Compose screens near `@shared/src/commonMain/kotlin/App.kt` until the module gains a richer package structure. Keep Android resources under `androidApp/src/androidMain/res/` and shared Compose resources under `shared/src/commonMain/resources/`.

## Platform Boundaries

Use Kotlin `expect`/`actual` for platform differences, following `getPlatformName()` in `@shared/src/commonMain/kotlin/App.kt` and the platform source sets. Do not import Android-only APIs from `commonMain`.

## Testing

No test source sets are present yet. Before handing off mobile changes, run the narrow Gradle task for the touched app and `./gradlew build` when the change crosses module boundaries.
