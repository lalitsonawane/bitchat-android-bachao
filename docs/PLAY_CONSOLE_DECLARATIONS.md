# Play Console Declaration Draft

Draft answers aligned with the current app (`com.bitchat.droid`) and [`PRIVACY_POLICY.md`](../PRIVACY_POLICY.md).  
Reviewers should verify against the live build before submitting.

## App identity

- **Package name:** `com.bitchat.droid`
- **App name:** bitchat
- **Category:** Communication
- **Free / paid:** Free
- **Contains ads:** No
- **In-app purchases:** No

## Privacy policy

**URL:** https://github.com/permissionlesstech/bitchat-android/blob/main/PRIVACY_POLICY.md

## Data safety (draft)

### Data collection

| Question | Draft answer |
|----------|--------------|
| Does your app collect or share any of the required user data types? | **No data collected** by the developer |
| Is all user data encrypted in transit? | N/A for developer-collected data; peer/relay traffic uses encryption appropriate to the mode (Noise/E2E for private mesh messages; Nostr transport for geohash) |
| Do you provide a way for users to request data deletion? | Users wipe local data in-app (emergency wipe / uninstall). No developer-held account data |

### Important nuance (declare carefully)

The developer does **not** operate an analytics backend or user database. The app does process data **on-device** and may transmit:

- Ephemeral peer identifiers / nicknames to nearby mesh peers (Bluetooth / Wi‑Fi Aware)
- Coarse **geohash** (not precise GPS) to public Nostr relays when geohash chat is used
- Message content to peers or relays chosen by the user

If Play Console wording requires declaring “data processed by the app” even when not collected by the developer, prefer honesty over the absolute “no data” shortcut. The live listing currently states “No data collected / No data shared with third parties”; keep that only if it still matches your legal interpretation.

Suggested explicit declarations if you expand beyond “no data collected”:

| Data type | Collected by developer? | Shared with developer? | Purpose | Notes |
|-----------|-------------------------|------------------------|---------|-------|
| Approximate location (geohash) | No | No | App functionality | Computed locally; coarse geohash may be published to Nostr relays user connects to |
| Precise location | No | No | App functionality | Used locally for BLE / geohash derivation; not uploaded to developer |
| Messages / chat content | No | No | App functionality | Delivered to peers/relays; not stored by developer |
| Photos / files / audio | No | No | App functionality | User-initiated sharing with peers |
| Device or other IDs | No | No | App functionality | Ephemeral/cryptographic keys stay on device |

## Sensitive permissions — justification draft

### Location (fine / coarse)

- **Why:** Android requires location permission for BLE scanning on many API levels; geohash channels need location to derive a coarse region.
- **How:** Processed on-device. Precise GPS is never sent to a bitchat developer server.
- **Background location:** Used so mesh discovery can continue while the app is not in the foreground (foreground service + BLE scanning). Not used for advertising or analytics.

Suggested Play answer theme: *“Background location is required for Bluetooth mesh peer discovery while the app runs as a foreground service. Location is not collected or sold.”*

### Nearby devices / Bluetooth

- **Why:** Core offline mesh transport (advertise, scan, connect).
- **User-facing disclosure:** Shown during onboarding permission explanation screens.

### Microphone (`RECORD_AUDIO`)

- **Why:** Optional voice notes.
- **When:** Only when the user records a voice message.

### Camera

- **Why:** QR code verification / pairing flows.
- **When:** Only when the user opens QR verification.

### Notifications

- **Why:** Alert users to private messages.
- **Optional:** Requested as optional permission on Android 13+.

### Photos / media read

- **Why:** User-initiated file/image sharing.
- **When:** When the user picks media to send.

### Foreground services

Declared types in the manifest:

- `connectedDevice` — BLE mesh connections
- `dataSync` — mesh/message sync work
- `location` — background BLE scanning that depends on location APIs

### Battery optimization exemption

- **Why:** Improves reliability of background mesh; requested with user consent / system dialog.
- Not used to run hidden tracking.

## Content rating (draft)

- App category: social / messaging style communication
- Users can communicate with each other (Users Interact)
- No targeted advertising
- User-generated text (and optional media) may be shared with peers/relays
- Recommend completing IARC questionnaire as **Everyone** with user interaction, matching the current store listing, unless store policy guidance changes

## Ads / financial

- Contains ads: **No**
- Paid app: **No**
- In-app purchases: **No**
- Gambling: **No**

## Government / news / COVID / etc.

- Not a news app
- Not a COVID contact-tracing app
- Not affiliated with a government agency

## Store listing copy (sync with Fastlane)

Keep Play Console text aligned with:

- `fastlane/metadata/android/en-US/short_description.txt`
- `fastlane/metadata/android/en-US/full_description.txt`

## Pre-submit policy checks

- [ ] Background location video/script ready if Play asks for a demo
- [ ] Permission denial paths still allow limited use where feasible (mesh needs BLE; geohash needs location)
- [ ] No misleading “works fully offline” claims that ignore geohash/Nostr internet features
- [ ] Security disclaimer remains accurate (no external security audit yet — see README warning)
