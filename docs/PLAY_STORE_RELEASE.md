# Google Play Store Release Checklist

Official listing: [`com.bitchat.droid`](https://play.google.com/store/apps/details?id=com.bitchat.droid)  
Publisher authorization: [`GOOGLE_PLAY.md`](../GOOGLE_PLAY.md)  
Privacy policy URL: https://github.com/permissionlesstech/bitchat-android/blob/main/PRIVACY_POLICY.md  
Play Console declaration draft: [`PLAY_CONSOLE_DECLARATIONS.md`](PLAY_CONSOLE_DECLARATIONS.md)

## Who can publish

| Goal | What you need |
|------|----------------|
| Update the official app | Access to the Verse Communication PBC Play Console account (or collaborator access) and the existing upload key |
| Publish a fork | New `applicationId` (package name), your own Play developer account, new signing keys, and your own hosted privacy policy |

`com.bitchat.droid` is already taken. Do not attempt a second listing with the same package name.

## Pre-flight (code)

- [ ] Bump `versionCode` (integer, must increase) and `versionName` in `app/build.gradle.kts`
- [ ] Add release notes under `fastlane/metadata/android/en-US/changelogs/<versionCode>.txt`
- [ ] Confirm `targetSdk` meets current Play requirements (currently 35)
- [ ] Smoke-test a release build on a physical device (BLE mesh, permissions, geohash, notifications)
- [ ] Confirm minify/ProGuard still works: `./gradlew assembleRelease`
- [ ] Prefer an App Bundle for Play: `./gradlew bundleRelease`

## Signing

Release signing is optional at configure time. When credentials are present, the `release` build type uses them automatically.

1. Create or obtain the Play **upload** keystore (never commit it).
2. Copy the template:

   ```bash
   cp keystore.properties.example keystore.properties
   ```

3. Fill `storeFile`, `storePassword`, `keyAlias`, and `keyPassword`.
4. Build:

   ```bash
   ./gradlew bundleRelease
   ```

   Output: `app/build/outputs/bundle/release/app-release.aab`

Environment-variable alternative (CI):

```bash
export BITCHAT_STORE_FILE=/secure/path/upload.jks
export BITCHAT_STORE_PASSWORD=...
export BITCHAT_KEY_ALIAS=...
export BITCHAT_KEY_PASSWORD=...
./gradlew bundleRelease
```

Without credentials, release builds remain unsigned (current GitHub Releases workflow behavior).

## Store listing assets (Fastlane)

Metadata lives under `fastlane/metadata/android/en-US/`.

| Asset | Path | Requirement |
|-------|------|-------------|
| Short description | `short_description.txt` | ≤ 80 characters |
| Full description | `full_description.txt` | ≤ 4000 characters |
| App icon | `images/icon.png` | 512 × 512 PNG |
| Feature graphic | `images/featureGraphic/` | 1024 × 500 PNG (see placeholder README) |
| Phone screenshots | `images/phoneScreenshots/` | 2–8 images (see placeholder README) |
| Changelog | `changelogs/<versionCode>.txt` | Per-release notes |

## Play Console forms

Complete or verify before every sensitive-permission release:

- [ ] Privacy policy URL (public HTTPS page)
- [ ] Data safety form — see declaration draft
- [ ] Permissions / Data safety justifications for location, Bluetooth, mic, camera, notifications
- [ ] Background location declaration (core mesh reliability)
- [ ] Foreground service declarations (`connectedDevice`, `dataSync`, `location`)
- [ ] Content rating questionnaire (users interact / messaging)
- [ ] Ads declaration (no ads)
- [ ] Target audience and news-app declarations as applicable

## Upload steps

1. Open Play Console → bitchat → Production (or testing track).
2. Create release → upload `app-release.aab`.
3. Paste release notes from Fastlane changelog.
4. Review pre-launch report / policy warnings.
5. Submit for review.

## Fork-only extras

If publishing independently (not the official listing):

- [ ] Change `applicationId` in `app/build.gradle.kts`
- [ ] Host your own privacy policy URL and update `PRIVACY_POLICY.md` contact links
- [ ] Create a new upload key + enable Play App Signing
- [ ] Do not reuse Verse/official branding claims without authorization
