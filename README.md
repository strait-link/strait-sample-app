# Bridge — sample app (Expo / React Native)

A dummy "Acme" app to test Bridge end-to-end on a real phone, including the
**native deep-link verification** (Android App Links / iOS Universal Links via
`/.well-known/assetlinks.json` + `apple-app-site-association`).

It's pre-wired to verify against the Bridge host
`bridge-redirect-engine.onrender.com` (see `app.json` → `intentFilters` /
`associatedDomains`).

---

## Level 1 — quick test in Expo Go (no build, ~10 min)
Proves the SDK + match API + events. **Does NOT test App-Links verification**
(Expo Go isn't your signed app, so the OS won't hand it verified links).

```sh
npm install
npx expo start         # scan the QR with Expo Go (iOS Camera / Android Expo Go)
```
Paste your App ID → tap "Check for a deferred link", "signup", "purchase".
Verify in Dashboard → Analytics.

## Level 2 — real App-Links verification (Android, no paid account)
This is the prod-readiness test: tap a Bridge link → the OS opens THIS app
directly (no browser bounce), because `assetlinks.json` lists your build's cert.

1. **Build a standalone APK** (cloud, free):
   ```sh
   npm i -g eas-cli && eas login        # free Expo account
   eas build -p android --profile preview
   ```
   (or local: `npx expo run:android` on a machine with Android SDK)
2. **Get the build's SHA-256 fingerprint** — EAS prints it, or:
   ```sh
   eas credentials       # → Android → shows the SHA-256
   ```
3. **Send me that SHA-256** → I put it in your Bridge app config so
   `https://bridge-redirect-engine.onrender.com/.well-known/assetlinks.json`
   lists `com.bridge.sample` + your fingerprint.
4. **Install the APK** on a real Android phone.
5. **Tap a Bridge link** (open it in another app / messaging). Android checks
   assetlinks → opens Acme directly → the green "Opened via link" banner shows.

## Level 3 — iOS Universal Links (needs Apple Developer account, $99/yr)
Same idea; requires the Associated Domains entitlement on a signed build:
```sh
eas build -p ios --profile preview   # needs Apple Developer credentials
```
Then I configure your Team ID + bundle in Bridge so
`apple-app-site-association` is valid, you install via TestFlight, and tap a link.

---

## What proves what
| Test | Proves |
|---|---|
| Expo Go (L1) | SDK + /v1/match + events + analytics |
| Android standalone (L2) | App Links verification via assetlinks.json (the moat, no paid acct) |
| iOS standalone (L3) | Universal Links via apple-app-site-association |
