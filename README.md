# CalorieDash — Android app (Trusted Web Activity)

The Android app for [caloriedash.com](https://www.caloriedash.com): a Bubblewrap
Trusted Web Activity project (package `com.caloriedash.app`) that opens the live
site fullscreen. Website updates flow into the app automatically — rebuilds are
only needed for store listing changes (icon, name, version bump).

## How the build works

- The GitHub Actions workflow (`.github/workflows/build.yml`) builds the
  **unsigned** release APK and AAB on every push to `main`, or on demand from
  the Actions tab ("Run workflow").
- Download the `caloriedash-android-unsigned` artifact from the finished run.
- Signing is done locally by the owner with the upload keystore
  (`caloriedash-upload.keystore`, kept offline — never commit it here):
  - APK: `zipalign` then `apksigner sign`
  - AAB: `jarsigner -signedjar`
- Upload the signed `.aab` in Google Play Console.

## Before the app opens without browser bars

`assetlinks.json` (Digital Asset Links, containing the signing certificate's
SHA-256 fingerprint) must be hosted at
`https://www.caloriedash.com/.well-known/assetlinks.json`.
If Play App Signing re-signs the app, Google's release fingerprint must be in
that file too.
