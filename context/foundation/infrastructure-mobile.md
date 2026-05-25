---
project: zwierzatko-mobile
researched_at: 2026-05-25T00:49:32+02:00
recommended_platform: Firebase App Distribution + Google Play Console
runner_up: GitHub Actions + Gradle + Firebase/Play API
context_type: mvp
tech_stack:
  language: Kotlin
  framework: Compose Multiplatform
  runtime: Android-first Gradle app
backend_contract:
  auth: Supabase
  database: Supabase
  owner: web stack
---

## Recommendation

**Use Supabase as the shared backend, Firebase App Distribution for tester builds, and Google Play Console for Android release tracks.**

The mobile app is Android-first Kotlin Compose Multiplatform and is a companion to the web MVP, not a separate backend product. Supabase should remain the single auth and data contract shared with `web/`; Firebase is recommended only for mobile distribution and optional diagnostics, while Google Play Console is the public Android distribution target.

## Platform Comparison

| Platform | CLI-first | Managed/Serverless | Agent-readable docs | Stable deploy API | MCP / Integration | Total |
|---|---|---|---|---|---|---|
| Firebase App Distribution + Google Play Console | Pass | Pass | Pass | Partial | Partial | 4/5 |
| GitHub Actions + Gradle + Firebase/Play API | Pass | Partial | Pass | Pass | Pass | 4/5 |
| Codemagic | Pass | Pass | Pass | Pass | Partial | 4/5 |
| Bitrise | Pass | Pass | Pass | Pass | Partial | 4/5 |
| App Store Connect + TestFlight | Partial | Pass | Pass | Pass | Fail | 3/5 |
| EAS Build/Submit | Pass | Pass | Pass | Pass | Partial | 3/5 |

Firebase App Distribution + Google Play Console: best fit for the current Android-first MVP because it is familiar, low-cost, and keeps release mechanics inside Google's Android ecosystem. Firebase App Distribution handles tester access through CLI/API, while Play Console handles internal, closed, open, and production release tracks. It does not replace Supabase.

GitHub Actions + Gradle + Firebase/Play API: strong runner-up because it can build with the checked-in Gradle wrapper and publish artifacts through service accounts. It is better after the manual signing and distribution flow is proven once locally.

Codemagic: strong mobile CI/CD platform with KMP support and good release ergonomics. It loses for this MVP because the user prioritized minimal cost and the project is a solo, Android-first experiment.

Bitrise: mature mobile CI/CD and Google Play deployment support. It is heavier and more expensive than this MVP needs before release flow is validated.

App Store Connect + TestFlight: required later for iOS distribution, but not the first deployment decision because `tech-stack-mobile.md` explicitly targets Android first.

EAS Build/Submit: excellent for Expo/React Native, but this project is Kotlin Compose Multiplatform. It is not a natural fit for the current stack.

### Shortlisted Platforms

#### 1. Firebase App Distribution + Google Play Console (Recommended)

It wins because it separates the right concerns: Supabase remains the shared backend, Firebase distributes Android test builds, and Google Play Console owns public Android release tracks. It also matches the user's familiarity with Google Play/Firebase and the minimal-cost constraint.

#### 2. GitHub Actions + Gradle + Firebase/Play API

This is the automation path after the first manual release loop works. It can run Gradle builds, upload artifacts, and keep logs in GitHub, but it introduces signing and service-account risk earlier.

#### 3. Codemagic

Codemagic is the paid DX option for mobile builds and future iOS expansion. It is useful when local/Actions-based builds become slow or when iOS signing becomes a bottleneck.

## Anti-Bias Cross-Check: Firebase App Distribution + Google Play Console

### Devil's Advocate - Weaknesses

1. Firebase App Distribution and Google Play Console solve different jobs. Treating Firebase as a full release platform would delay the real Play Console setup.
2. Firebase must not become a second auth/database backend. Supabase is the source of truth for user identity, pet ownership, and health-history data.
3. Android signing is the highest-risk operational step. Losing or rotating keys incorrectly can block updates.
4. First Play Console setup includes manual gates: developer account, app signing, store listing, countries, privacy policy, testing tracks, and review.
5. Firebase App Distribution builds expire after 150 days, so they are not a long-term release archive.

### Pre-Mortem - How This Could Fail

Six months after starting mobile, the team discovers that the Android app has drifted from the web backend. A quick Firebase setup was treated as permission to use Firebase Auth or Firestore for mobile-only shortcuts, so web and mobile now disagree on ownership rules and user sessions. Test builds were easy to send through Firebase App Distribution, but no one completed the Google Play Console release track setup until late. When production release finally starts, signing configuration, privacy policy, package name, and internal testing rules block progress. Older Firebase test builds have expired, making regression comparison harder. The failure was not Firebase itself; it was using a test-distribution tool as a backend and release strategy instead of keeping Supabase as the data contract and Play Console as the real distribution gate.

### Unknown Unknowns

- Firebase App Distribution is for tester builds, not public store distribution.
- Supabase auth/session support for Kotlin Multiplatform must be validated before building mobile auth screens.
- Google Play first-release requirements can differ by account type and policy state.
- `.aab` is the preferred Play artifact, while APKs are often easier for local/test distribution.
- iOS/TestFlight remains a separate pipeline with macOS/Xcode/App Store Connect requirements.

## Operational Story

- **Preview deploys**: Build local or CI Android artifacts from `multiplatform/` and distribute tester builds through Firebase App Distribution groups.
- **Secrets**: Store signing material and Google/Firebase service-account credentials outside the repo. Use local keystore files for manual builds first; later move CI secrets into GitHub Actions or the chosen mobile CI.
- **Rollback**: For Firebase App Distribution, send testers a previous build if it has not expired. For Google Play, halt or reduce rollout and promote a known-good version through the appropriate track; database rollback remains a Supabase concern.
- **Approval**: An agent may run Gradle builds and prepare release notes. A human approves signing-key setup, Play Console account/policy changes, production rollout, Supabase schema changes, and any destructive data action.
- **Logs**: Use Gradle output for build failures, Firebase App Distribution release status for tester delivery, and Play Console status for track/review state. Add Crashlytics later only if runtime crash feedback becomes necessary.

## Risk Register

| Risk | Source | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| Mobile accidentally uses Firebase Auth/Firestore instead of Supabase | Devil's advocate | M | H | State in mobile AGENTS/plans that Supabase is the only auth/data backend unless explicitly redesigned. |
| Android signing key is lost or misconfigured | Pre-mortem | M | H | Document keystore location, backup, passwords, and Play App Signing setup before first release. |
| Firebase test builds expire before regression checks | Unknown unknowns | M | M | Keep release notes and build artifacts for important milestones outside Firebase if needed. |
| Google Play first-release setup blocks launch | Research finding | M | H | Complete internal testing track, app signing, privacy policy, and store listing before feature-complete MVP. |
| CI service account has excessive publish permissions | Devil's advocate | L | H | Start manual; later create least-privilege service accounts scoped to testing tracks before production. |
| iOS expectations leak into Android MVP | Unknown unknowns | M | M | Treat iOS/TestFlight as a later infrastructure decision unless Android MVP is stable. |

## Getting Started

1. From `multiplatform/`, build the Android debug app with `./gradlew :androidApp:assembleDebug`.
2. Configure the Android package name and signing plan before any public or Play Console build; the current application id is `com.myapplication.MyApplication`.
3. Create a Firebase project only for distribution/diagnostics and register the Android app there; do not add Firebase Auth or Firestore as app data backends.
4. Install Firebase CLI if needed and distribute a debug/release APK to testers with `firebase appdistribution:distribute`.
5. Set up Google Play Console internal testing once the app has a signed AAB and store-policy basics are ready.

## Sources Checked

- Firebase App Distribution CLI: https://firebase.google.com/docs/app-distribution/android/distribute-cli
- Firebase CLI reference: https://firebase.google.com/docs/cli
- Firebase pricing plans: https://firebase.google.com/docs/projects/billing/firebase-pricing-plans
- Google Play release tracks: https://support.google.com/googleplay/android-developer/answer/9859348
- fastlane supply / Google Play upload: https://docs.fastlane.tools/actions/supply/
- Gradle GitHub Actions setup: https://github.com/gradle/actions/blob/main/docs/setup-gradle.md
- Bitrise Android deployment docs: https://docs.bitrise.io/en/bitrise-ci/deploying/android-deployment/deploying-android-apps-to-bitrise-and-google-play.html
- App Store Connect API: https://developer.apple.com/app-store-connect/api/

## Out of Scope

The following were not evaluated in this research:

- Docker image configuration
- CI/CD pipeline setup
- Production-scale architecture
- iOS/TestFlight release implementation
