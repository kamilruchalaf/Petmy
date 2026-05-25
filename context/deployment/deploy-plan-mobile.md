# Mobile Deployment Plan

## Summary

Deploy the `multiplatform/` Android application to the Google Play Console internal testing track.

The first mobile deployment uses an Android App Bundle (`.aab`) built from the checked-in Gradle wrapper. Supabase remains the shared auth and data backend owned by the web stack. Firebase Auth, Firestore, Crashlytics, and Firebase App Distribution are out of scope for this deployment.

## Deployment Stage Checklist

- [ ] Stage 1: Android project identity configured.
- [ ] Stage 2: Release signing configured.
- [ ] Stage 3: Local debug build verified.
- [ ] Stage 4: Local release bundle verified.
- [ ] Stage 5: Play Console app prepared.
- [ ] Stage 6: Google Play service account configured.
- [ ] Stage 7: GitHub Actions secrets configured.
- [ ] Stage 8: CI release workflow added.
- [ ] Stage 9: Internal track upload completed.
- [ ] Stage 10: Tester install smoke test completed.

## Stage 1: Android Project Identity

- [ ] Set the Android application id to `pl.droidcode.apps.petmy`.
- [ ] Set the visible app name to `Petmy`.
- [ ] Keep the first release version at `versionCode = 1` and `versionName = "1.0"` unless Google Play rejects the version code as already used.

Acceptance:

- [ ] `multiplatform/androidApp/build.gradle.kts` uses `applicationId = "pl.droidcode.apps.petmy"`.
- [ ] `multiplatform/androidApp/src/androidMain/res/values/strings.xml` uses `Petmy` for `app_name`.

## Stage 2: Release Signing

- [ ] Generate or select the Android release keystore outside the repository.
- [ ] Store keystore path, alias, passwords, backup location, and owner outside the repository.
- [ ] Configure Gradle release signing from local properties or environment variables.
- [ ] Do not commit keystore files, passwords, service-account JSON files, or generated secret material.

Acceptance:

- [ ] Release signing works locally without tracked secret files.
- [ ] `./gradlew :androidApp:signingReport` shows the expected release signing configuration.

## Stage 3: Local Debug Build

Run all commands from `multiplatform/`.

```bash
./gradlew :androidApp:assembleDebug
```

Acceptance:

- [ ] The debug build passes.
- [ ] Android SDK Platform 34 and required build tools are installed.
- [ ] Android SDK licenses are accepted on the build machine.

## Stage 4: Local Release Bundle

Run all commands from `multiplatform/`.

```bash
./gradlew :androidApp:lintVitalRelease
./gradlew :androidApp:bundleRelease
```

Expected artifact:

```text
multiplatform/androidApp/build/outputs/bundle/release/androidApp-release.aab
```

Acceptance:

- [ ] Release lint passes.
- [ ] Release bundle build passes.
- [ ] The `.aab` exists at the expected path.

## Stage 5: Play Console App

- [ ] Create the Android app in Google Play Console with package id `pl.droidcode.apps.petmy`.
- [ ] Enable Play App Signing.
- [ ] Complete the minimum required store listing fields.
- [ ] Complete privacy policy, content rating, data safety, and target audience requirements.
- [ ] Create or select the internal testing track.
- [ ] Add the initial internal tester group.

Acceptance:

- [ ] Google Play Console accepts the package id.
- [ ] Internal testing is available for the app.
- [ ] The app is not promoted to production.

## Stage 6: Google Play Service Account

- [ ] Enable Google Play Developer API access for the Play Console account.
- [ ] Create a service account for CI publishing.
- [ ] Grant the service account the minimum permissions needed to publish this app to the internal track.
- [ ] Download the service-account JSON once and store it as a GitHub secret, not as a repository file.

Acceptance:

- [ ] The service account can access only the intended app and release capability.
- [ ] No service-account JSON is committed to the repository.

## Stage 7: GitHub Actions Secrets

Configure these GitHub repository secrets:

- [ ] `ANDROID_RELEASE_KEYSTORE_BASE64`
- [ ] `ANDROID_RELEASE_STORE_PASSWORD`
- [ ] `ANDROID_RELEASE_KEY_ALIAS`
- [ ] `ANDROID_RELEASE_KEY_PASSWORD`
- [ ] `GOOGLE_PLAY_SERVICE_ACCOUNT_JSON`

Acceptance:

- [ ] CI can reconstruct the keystore during a workflow run.
- [ ] CI can authenticate to Google Play without printing secret values.

## Stage 8: CI Release Workflow

Add a GitHub Actions workflow for mobile internal releases.

The workflow should:

- [ ] Check out the repository.
- [ ] Set up JDK 17.
- [ ] Decode the Android release keystore from secrets.
- [ ] Run `./gradlew :androidApp:lintVitalRelease` from `multiplatform/`.
- [ ] Run `./gradlew :androidApp:bundleRelease` from `multiplatform/`.
- [ ] Upload the generated `.aab` as a GitHub Actions artifact.
- [ ] Upload the `.aab` to Google Play internal testing.

Recommended upload implementation:

- Use `r0adkll/upload-google-play` for the first automated internal release.
- Consider `fastlane supply` later if metadata, screenshots, changelogs, or track promotions need to be automated.

Acceptance:

- [ ] GitHub Actions shows clear deploy-stage step names.
- [ ] Failed stages stop the workflow before publishing.
- [ ] The upload step targets only the `internal` track.

## Stage 9: Internal Track Upload

- [ ] Upload `androidApp-release.aab` to the Google Play internal testing track.
- [ ] Add concise release notes.
- [ ] Submit the internal release for tester availability.

Acceptance:

- [ ] Google Play accepts the bundle.
- [ ] There is no signing key, package id, or version code conflict.
- [ ] The internal release is visible in Play Console.

## Stage 10: Tester Smoke Test

- [ ] Open the internal testing opt-in link as a tester.
- [ ] Install the app from Google Play.
- [ ] Launch the app.
- [ ] Confirm the initial Compose screen renders.
- [ ] Confirm there is no crash on first open.

Acceptance:

- [ ] At least one tester can install the internal release.
- [ ] The app opens successfully from the installed Google Play build.

## Rollback Notes

- Internal testing rollback is handled by uploading or promoting a known-good bundle with a higher `versionCode`.
- Google Play does not allow reusing an older `versionCode` for a new upload.
- Data rollback remains a Supabase concern and is not covered by Android release rollback.
- Production rollout is out of scope for this plan.

## Assumptions

- The target package id is `pl.droidcode.apps.petmy`.
- The target Google Play track is `internal`.
- The first automated path is GitHub Actions plus Google Play Developer API.
- The first Play Console app setup is manual.
- iOS and TestFlight are out of scope.
- Firebase is not used for this Google Play internal deployment.
